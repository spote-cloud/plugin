# Spote plugin for ChatGPT and Claude — design

## Goal

Package the existing remote Spote MCP server (`https://spote.cloud/mcp`) as an
installable plugin for both ChatGPT/Codex and Claude Code, bundled with the
"Spote usage" skill (Spote note `9f12b155-f13f-4439-8f83-a716f913e446`).

No server code is written or hosted here — this repo is pure packaging
(manifests + config) around the existing hosted MCP endpoint, per Spote note
`d6e6831e-d4a7-40a7-bb4d-865684a3d236`.

## File layout

```
plugin/
├── README.md
├── plugin.json                        ChatGPT/Codex manifest (root)
├── mcp.json                           ChatGPT MCP config → spote.cloud/mcp
├── .claude-plugin/
│   ├── plugin.json                    Claude Code manifest
│   └── marketplace.json               self-hosted marketplace (source: "./")
├── .mcp.json                          Claude MCP config → spote.cloud/mcp
└── skills/
    └── spote-usage/
        └── SKILL.md                   bundled skill content
```

Both manifest pairs live in the same repo root since their filenames don't
collide (`plugin.json` vs `.claude-plugin/plugin.json`, `mcp.json` vs
`.mcp.json`). The `skills/` directory is shared and auto-discovered by
Claude Code; ChatGPT's plugin format also supports a `skills/` directory of
`SKILL.md` files.

## Components

**`.mcp.json` (Claude) / `mcp.json` (ChatGPT)**
Both declare one remote MCP server, `spote`, pointing at
`https://spote.cloud/mcp`. Claude uses `"type": "http"`; ChatGPT uses
`"type": "streamable-http"`. No headers/auth config here — the hosted
server handles its own OAuth per client.

**`.claude-plugin/plugin.json`**
`name: "spote"`, description, `author: {name: "Spote", url: "https://spote.cloud"}`,
`repository: "https://github.com/spote-cloud/plugin"`,
`homepage: "https://spote.cloud"`, `license: "MIT"`,
`mcpServers: "./.mcp.json"`. Skills are auto-discovered from `skills/`, so no
explicit skills field is required.

**`plugin.json` (root, ChatGPT/Codex)**
Same core metadata (name, version, description, author) plus an
`extensions.com.openai.interface` block with `displayName: "Spote"` and
`category: "Productivity"`.

**`.claude-plugin/marketplace.json`**
Self-hosted marketplace: `plugins: [{ name: "spote", source: "./", ... }]`,
so `/plugin marketplace add spote-cloud/plugin` followed by
`/plugin install @spote-marketplace spote` installs this same repo as a
plugin.

**`skills/spote-usage/SKILL.md`**
Content of Spote note `9f12b155-f13f-4439-8f83-a716f913e446` ("Spote usage
skill"), given standard SKILL.md frontmatter (`name`, `description`) and kept
otherwise as-is (search-before-create, avoid duplicate notes, Markdown
structure, hashtag tags, Mermaid for relationships, `relate_notes` for
durable links).

**`README.md`**
Short update: what the plugin is, the two install paths (Claude
`/plugin marketplace add`, ChatGPT connector/marketplace), and a pointer to
`spote.cloud`.

## Out of scope

- No server code, no CI, no icon/asset files (ChatGPT icon field omitted).
- No public submission to ChatGPT's plugin directory (that's a manual,
  separate review process per the source note) — this repo only enables
  self-hosted / git-based installation.
- No automated tests — the deliverables are static config/manifest files;
  correctness is checked by validating JSON syntax and cross-referencing the
  schema confirmed via the claude-code-guide agent.

## Testing / validation

- `python3 -m json.tool` (or equivalent) on every `.json` file to catch
  syntax errors.
- Manual read-through of `SKILL.md` frontmatter against Claude's SKILL.md
  convention.
