# MEMBER.md

A format for defining complete AI team members. Composes existing standards ([SKILL.md](https://agentskills.io), [MCP](https://modelcontextprotocol.io), [AGENTS.md](https://github.com/anthropics/agents-md)) into a single deployable unit.

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

A member directory is self-contained. Zip it, hand it to someone, they drop it in their project. Every file inside uses formats that existing tools already read.

# About This Repository

This repository contains the MEMBER.md format and example team members that demonstrate what's possible. Members range from dev workflow agents (navigator, executor, validator) to business role agents (SDR with skills, tools, and domain context).

Each member is self-contained in its own folder with a `MEMBER.md` file containing the metadata and instructions that AI agents use. Browse through these members to get inspiration for your own team or to understand different patterns.

- [./team](./team): Example team members
- [./template](./template): Member template

# Quick Start

## Creating a member

1. Create a `team/` directory in your project
2. Add a subdirectory per member (e.g., `team/sdr/`)
3. Write a `MEMBER.md` with frontmatter and instructions

You can use the [template](./template/MEMBER.md) as a starting point:

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
  - hubspot              # MCP server (tools/.mcp.json)
  - apollo               # REST API (tools/apollo.md)
---
```

Below the frontmatter: persona, workflow, and rules the member follows.

The frontmatter requires only two fields:
- `name` — kebab-case identifier for this member
- `role` — human-readable job title

Optional fields: `description`, `version`, `skills`, `permissions`, `tools`.

## Two formats

Members support a flat file for simple cases and a directory for full members:

**Flat file** — just a markdown file with frontmatter:
```
team/
├── navigator.md
├── executor.md
└── validator.md
```

**Directory** — when a member needs its own skills, tools, or context:
```
team/
└── sdr/
    ├── MEMBER.md
    ├── skills/
    │   └── email-outreach/SKILL.md
    ├── tools/
    │   ├── .mcp.json
    │   └── apollo.md
    └── context/
        └── playbook.md
```

Same frontmatter, same format. The directory just adds room for extras. Detection order: `team/<name>/MEMBER.md` first, `team/<name>.md` as fallback.

## Directory structure

```
<member-name>/
├── MEMBER.md              REQUIRED  Manifest + persona
├── skills/                OPTIONAL  SKILL.md files (capabilities)
│   └── <skill-name>/
│       └── SKILL.md
├── tools/                 OPTIONAL  Tool configurations
│   ├── .mcp.json                    MCP servers
│   └── *.md                         API references, CLI docs
└── context/               OPTIONAL  Domain knowledge
    └── *.md
```

**skills/** — [SKILL.md](https://agentskills.io) files defining individual capabilities. A member composes multiple skills into a role.

**tools/** — Any tool access the member needs. MCP servers via `.mcp.json`, REST APIs as markdown docs with endpoints and auth, CLI tools as markdown docs with commands. Not everything is MCP.

**context/** — Markdown files with domain knowledge. Sales playbooks, ICPs, SOPs, customer docs. No special format — just docs the member references.

# Using Members with AI Tools

MEMBER.md is markdown. Every AI coding tool already reads markdown. No platform changes required.

## Claude Code

Add to your project's CLAUDE.md:

```markdown
## Team

This project uses MEMBER.md team members in `team/`.

When activated as a specific member:
1. Read `team/<name>/MEMBER.md` for persona, role, and permissions
2. Load skills from `team/<name>/skills/`
3. Load context from `team/<name>/context/`
4. Use tools from `team/<name>/tools/`
5. Operate within declared permissions
```

**Activate directly:**
```markdown
You are the **navigator**. Read `team/navigator/MEMBER.md` before doing anything.
```

**Multi-agent via subagents:**
```
Main agent (reads CLAUDE.md)
├── Task: "Act as navigator — read team/navigator/MEMBER.md, plan this feature"
├── Task: "Act as executor — read team/executor/MEMBER.md, build from the plan"
└── Task: "Act as validator — read team/validator/MEMBER.md, review the changes"
```

Each subagent loads a different MEMBER.md. Different persona, different skills, different permissions.

## Codex

Add to your project's AGENTS.md:

```markdown
## Team Members

This project defines AI team members in `team/*/MEMBER.md`.
When assigned a role, read the corresponding MEMBER.md first.
```

```
codex "As the support agent (see team/support/MEMBER.md), triage these new issues"
```

## Cursor

Reference members in `.cursorrules`:

```markdown
This project has AI team members defined in team/*/MEMBER.md.
When working on support tasks, reference team/support/MEMBER.md.
```

## Any tool

Any AI tool that reads markdown can use members. The convention:

1. Scan `team/*/MEMBER.md` (directory) and `team/*.md` (flat file)
2. Parse YAML frontmatter for metadata
3. Load skills, tools, and context from subdirectories
4. Follow the persona and rules in the body

No special parser. It's directories and markdown all the way down.

# Principles

- **Compose, don't compete.** MEMBER.md uses existing standards. It doesn't reinvent them.
- **A directory, not a format.** The "spec" is a folder convention. No new parsers needed.
- **Portable by default.** Zip a member directory, it works anywhere.
- **Tool-agnostic.** Nothing in the spec is tied to a specific AI tool or vendor.

# License

MIT
