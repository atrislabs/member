# MEMBER.md

A format for defining complete AI team members.

```
team/
├── sdr/
│   ├── MEMBER.md           ← persona, role, permissions
│   ├── skills/             ← SKILL.md files (capabilities)
│   ├── tools/              ← MCP servers, APIs, CLIs
│   └── context/            ← domain knowledge
└── support/
    ├── MEMBER.md
    ├── skills/
    ├── tools/
    └── context/
```

## The problem

AI tooling has standards for individual pieces:

| Standard | What it defines | Status |
|----------|----------------|--------|
| SKILL.md | A single capability | Adopted (Claude, Codex, Cursor) |
| .mcp.json | Tool server access | Adopted (Anthropic, OpenAI, Google) |
| AGENTS.md | Project instructions | Adopted (60,000+ repos) |

Nobody has a standard for the **bundle** — the thing that says "this AI worker has these skills, these tools, this persona, these permissions, and this context."

Today, teams cobble this together with system prompts, scattered files, and verbal instructions. There's no portable, shareable unit for "here's a complete AI worker."

## What MEMBER.md does

MEMBER.md is a directory convention that composes existing standards into a deployable AI team member.

It doesn't replace SKILL.md, .mcp.json, or AGENTS.md. It bundles them.

```
Project Level
├── CLAUDE.md / AGENTS.md        ← project scope
│
Team Level
├── team/sdr/MEMBER.md           ← member scope
│   ├── skills/SKILL.md          ← capabilities (existing standard)
│   ├── tools/                   ← MCP, APIs, CLIs (any tool access)
│   └── context/                 ← knowledge (just markdown files)
```

A member directory is self-contained. Zip it, hand it to someone, they drop it in their project. Every file inside uses existing formats that existing tools already read.

## Quick start

1. Create a `team/` directory in your project
2. Add a subdirectory per member (e.g., `team/sdr/`)
3. Write a `MEMBER.md` that defines persona, skills, and permissions
4. Add skills, tools, and context as needed

See [examples/](./examples/) for complete member definitions.

## MEMBER.md format

```yaml
---
name: sdr
role: Sales Development Rep
description: Outbound prospecting and lead qualification
version: 1.0.0

skills:
  - email-outreach        # local: ./skills/email-outreach/SKILL.md
  - calendar              # shared or external

permissions:
  can-draft: true
  can-send: false
  approval-required: [send, delete]

tools:
  - hubspot          # MCP server (tools/.mcp.json)
  - apollo           # REST API (tools/apollo.md)
  - linkedin         # browser tool (tools/linkedin.md)
---
```

Below the frontmatter: persona, workflow, and any instructions specific to this member.

See [spec.md](./spec.md) for the full specification.

## Detection

AI tools can detect members by scanning for `team/*/MEMBER.md` in a project. See [detection.md](./detection.md) for integration patterns with Claude Code, Cursor, Codex, and other tools.

## Principles

- **Compose, don't compete.** MEMBER.md uses existing standards. It doesn't reinvent them.
- **A directory, not a format.** The "spec" is a folder convention. No new parsers needed.
- **Portable by default.** Zip a member directory, it works anywhere.
- **Tool-agnostic.** Nothing in the spec is tied to a specific AI tool or vendor.

## License

MIT
