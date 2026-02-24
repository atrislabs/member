# MEMBER.md

A format for defining complete AI team members. Composes existing standards ([SKILL.md](https://agentskills.io), [AGENTS.md](https://github.com/anthropics/agents-md)) and any tool access (APIs, CLIs, MCP servers) into a single deployable unit.

```
team/
├── sdr/
│   ├── MEMBER.md           <- persona, role, permissions
│   ├── skills/             <- SKILL.md files (capabilities)
│   ├── tools/              <- APIs, CLIs, MCP servers
│   └── context/            <- domain knowledge
└── chief-of-staff/
    ├── MEMBER.md
    ├── skills/
    ├── context/
    └── journal/
```

## The problem

AI tooling has standards for individual pieces:

| Standard | What it defines | Status |
|----------|----------------|--------|
| SKILL.md | A single capability | Adopted (Claude, Codex, Cursor) |
| .mcp.json | Tool server config | Adopted (Anthropic, OpenAI, Google) |
| AGENTS.md | Project instructions | Adopted (60,000+ repos) |

Nobody has a standard for the **bundle** - the thing that says "this AI worker has these skills, these tools, this persona, these permissions, and this context."

Today, teams cobble this together with system prompts, scattered files, and verbal instructions. There's no portable, shareable unit for "here's a complete AI worker."

## What MEMBER.md does

MEMBER.md is a directory convention that composes existing standards into a deployable AI team member. It doesn't replace SKILL.md or AGENTS.md. It bundles them - along with whatever tools the member needs (REST APIs, CLIs, MCP servers, anything).

A member directory is self-contained. Zip it, hand it to someone, they drop it in their project. Every file inside uses formats that existing tools already read.

See [SPEC.md](./SPEC.md) for the full format specification.

## Quick start

1. Create a `team/` directory in your project
2. Add a subdirectory per member (e.g., `team/sdr/`)
3. Write a `MEMBER.md` with frontmatter and instructions

Use the [template](./template/MEMBER.md) as a starting point. Only two fields are required:

```yaml
---
name: sdr
role: Sales Development Rep
---
```

Below the frontmatter: persona, workflow, and rules the member follows. Add `skills/`, `tools/`, and `context/` subdirectories when the member needs its own capabilities or knowledge.

For simple members, a flat file works too -- `team/executor.md` instead of `team/executor/MEMBER.md`.

## Examples

| Member | Format | What it demonstrates |
|--------|--------|---------------------|
| [content-lead/](./team/content-lead/) | directory | **Showcase** - writing pipeline with skills, tools, and journal that tracks voice preferences |
| [chief-of-staff/](./team/chief-of-staff/) | directory | Stateful member with journal loop that learns over time |
| [sdr/](./team/sdr/) | directory | Full member with skills, tools, and context |
| [navigator/](./team/navigator/) | directory | Member with local skills, no tools or context |
| [spec-author/](./team/spec-author/) | directory | Member with context only, no skills or tools |
| [executor.md](./team/executor.md) | flat file | Flat file with abstract skill references |
| [validator.md](./team/validator.md) | flat file | Flat file with permissions and approval gates |
| [curator.md](./team/curator.md) | flat file | Minimal flat file, no skills or tools |

## Using members with AI tools

MEMBER.md is markdown. Every AI coding tool already reads markdown. No platform changes required.

### Claude Code

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

Activate directly:

```
You are the **navigator**. Read `team/navigator/MEMBER.md` before doing anything.
```

Multi-agent via subagents:

```
Main agent (reads CLAUDE.md)
├── Task: "Act as navigator - read team/navigator/MEMBER.md, plan this feature"
├── Task: "Act as executor - read team/executor.md, build from the plan"
└── Task: "Act as validator - read team/validator.md, review the changes"
```

### Codex

Add to your project's AGENTS.md:

```markdown
## Team Members

This project defines AI team members in `team/*/MEMBER.md`.
When assigned a role, read the corresponding MEMBER.md first.
```

```
codex "As the SDR (see team/sdr/MEMBER.md), qualify these inbound leads"
```

### Cursor

Reference members in `.cursorrules`:

```markdown
This project has AI team members defined in team/*/MEMBER.md.
When working on outreach tasks, reference team/sdr/MEMBER.md.
```

### Any tool

Any AI tool that reads markdown can use members:

1. Scan `team/*/MEMBER.md` (directory) and `team/*.md` (flat file)
2. Parse YAML frontmatter for metadata
3. Load skills, tools, and context from subdirectories
4. Follow the persona and rules in the body

No special parser. It's directories and markdown all the way down.

## License

MIT
