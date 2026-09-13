# Spote plugin

Packages the [Spote](https://spote.cloud) MCP server (`https://spote.cloud/mcp`) as an installable plugin for both Claude and ChatGPT/Codex, bundled with a skill for using Spote well.

No server code lives here — this repo only wires clients up to the existing hosted MCP endpoint.

## Install for Claude Code

```
/plugin marketplace add spote-cloud/plugin
/plugin install @spote-marketplace spote
```

Or add Spote directly as a connector in Claude.ai / Claude Desktop settings using the URL `https://spote.cloud/mcp` — no plugin install needed there.

## Install for ChatGPT / Codex

```
codex plugin marketplace add spote-cloud/plugin
```

Then install `spote` from that marketplace. (Public listing in ChatGPT's plugin directory is a separate, manual submission process — this repo only enables self-hosted / git-based installation.)

## What's bundled

- `.mcp.json` / `mcp.json` — MCP server config pointing at `https://spote.cloud/mcp` (Claude and ChatGPT formats respectively)
- `skills/spote-usage/SKILL.md` — guidance for searching, creating, tagging, and linking notes in Spote
- `.claude-plugin/plugin.json` / `plugin.json` — plugin manifests (Claude and ChatGPT/Codex respectively)
- `.claude-plugin/marketplace.json` — self-hosted marketplace listing so this repo can be added directly with `/plugin marketplace add`

