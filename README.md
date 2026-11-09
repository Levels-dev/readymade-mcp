# ready→made MCP: Figma to WordPress builders, from your AI assistant

> Your design is a spec, not a prompt.

ready→made is a hosted [MCP](https://modelcontextprotocol.io) server that turns a Figma
frame into a **native WordPress builder layout**: Elementor, Gutenberg, Oxygen 6,
Oxygen Classic, Etch, or plain HTML. A deterministic spatial layout engine computes it,
not a language model. Same frame in, same layout out, every time. No screenshots, no
hand-built sections, no guessed layouts.

**Live since 2026-10-04.**

It is the same engine as the [ready→made Figma plugin](https://wpconverters.com)
(900+ users on Figma Community), exposed to Claude Code, Claude Desktop, Cursor, Windsurf and VS Code.

This repository holds the **client configuration and docs only**. The server is closed
source and hosted at `https://api.levels.dev/mcp` (streamable HTTP).

Official MCP Registry: [`dev.levels/readymade`](https://registry.modelcontextprotocol.io/v0.1/servers?search=readymade)

## What you need

1. A ready→made **Pro** (€19/month) or **Lifetime** (€250) plan. MCP access is included;
   there are no free MCP conversions. [Pricing](https://wpconverters.com/pricing)
2. A **ready→made key**: [api.levels.dev/account](https://api.levels.dev/account)
   (log in with your plugin account, MCP section, Create key). The key is shown once.
3. A **Figma personal access token** with the *File content: read* scope:
   Figma → Settings → Security → Personal access tokens.

## Connect

**Claude Code**

```bash
claude mcp add --transport http readymade https://api.levels.dev/mcp \
  --header "Authorization: Bearer rm_live_…" \
  --header "x-figma-token: figd_…"
```

**Cursor**: paste this install link into your browser, then replace the two placeholders in Cursor's MCP settings:

```
cursor://anysphere.cursor-deeplink/mcp/install?name=readymade&config=eyJ1cmwiOiJodHRwczovL2FwaS5sZXZlbHMuZGV2L21jcCIsImhlYWRlcnMiOnsiQXV0aG9yaXphdGlvbiI6IkJlYXJlciBZT1VSX1JFQURZTUFERV9LRVkiLCJ4LWZpZ21hLXRva2VuIjoiWU9VUl9GSUdNQV9UT0tFTiJ9fQ==
```

or edit `~/.cursor/mcp.json`:

```json
{ "mcpServers": { "readymade": {
  "url": "https://api.levels.dev/mcp",
  "headers": { "Authorization": "Bearer rm_live_…", "x-figma-token": "figd_…" } } } }
```

**Windsurf** (`~/.codeium/windsurf/mcp_config.json`)

```json
{ "mcpServers": { "readymade": {
  "serverUrl": "https://api.levels.dev/mcp",
  "headers": { "Authorization": "Bearer rm_live_…", "x-figma-token": "figd_…" } } } }
```

**VS Code** (`.vscode/mcp.json`)

```json
{ "servers": { "readymade": { "type": "http",
  "url": "https://api.levels.dev/mcp",
  "headers": { "Authorization": "Bearer rm_live_…", "x-figma-token": "figd_…" } } } }
```

**Claude Desktop** (`claude_desktop_config.json`, via the `mcp-remote` bridge)

```json
{ "mcpServers": { "readymade": { "command": "npx", "args": [
  "-y", "mcp-remote", "https://api.levels.dev/mcp",
  "--header", "Authorization:Bearer rm_live_…",
  "--header", "x-figma-token:figd_…" ] } } }
```

## Tools

| tool | what it does | cost |
|---|---|---|
| `list_targets` | the six output formats with a paste/import note each | free |
| `inspect_figma` | outline of a file or frame: layers, sizes, counts, section candidates | free |
| `convert_figma` | one frame → one target; returns the paste-ready payload, how to use it, and whether the Figma data came from your cache | 1 conversion |

## Example prompts

* *"Convert the Pricing frame from https://www.figma.com/design/…?node-id=12-345 to Elementor and give me the JSON to paste."*
* *"Inspect this Figma file and list the sections on the Landing page. Then convert the hero to Gutenberg."*
* *"Convert node 5:21396 to Oxygen 6 and to HTML so I can compare."*

## Limits

* One **frame** per call (sections, not whole pages): up to 4,000 layers, 6 MB output, 90 s.
  Measured: a 266-layer section with 6 images converts in about 5 s the first time and
  about 0.5 s from your cache.
* 30 tool calls per minute per key, 300 conversions per day per account, 5 keys per account.
* Raster images are hosted on api.levels.dev for 24 hours and referenced by URL; Elementor copies them into your media library on paste/import (verified on 3.25.4 and 4.2.1). Ask for `options.assets: "inline"` to get data URLs instead (self-contained HTML/Gutenberg), or `"none"` to skip them. Up to 8 MB per image, 200 MB hosted per account per day.
* Figma-API friendly: frames you fetched are cached for your account for 7 days (per file version, never shared between accounts). Measured on a typical session (inspect + 5 conversions over 3 frames and 2 targets): 14 Figma API calls without the cache, 5 on the first session, 0 when repeated; `refresh: true` forces a refetch. That matters on View/Collab seats (Figma allows ~20 file reads per month there).
* Figma's own API rate limits apply to your token.
* Fonts: every result lists the families used and where they come from. Google fonts load automatically; a brand font (e.g. "Labil Grotesk") must be installed on the WordPress site. Until then the output falls back to a generic family of the same shape (Gutenberg/HTML emit `"Labil Grotesk", sans-serif`).

## Privacy

Your Figma token is sent with each request, forwarded to api.figma.com for that call and
never stored or logged. We store a hash of your MCP key and, per tool call, the tool name,
target, success flag, duration, payload size, layer count and MCP client name. No design content.

## Pasting into Elementor

Elementor output is cross-site paste JSON. In the Elementor editor, right-click an empty
area and choose **Paste from other site**, then paste. Works on Elementor 3 and 4. Images
are copied into your media library during the paste.

## Links

* Website: https://wpconverters.com/mcp
* Docs and setup: https://wpconverters.com/docs/mcp
* Pricing: https://wpconverters.com/pricing
* Figma plugin: https://wpconverters.com

## Support

support@levels.dev · [Docs](https://wpconverters.com/docs/mcp)

---
© Levels Branding & Webdevelopment OG. Configuration and documentation in this repository
are MIT; the conversion engine is proprietary and hosted.
