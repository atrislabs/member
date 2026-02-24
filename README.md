# MEMBER.md

A directory standard for defining AI agent team members in your project.

As frameworks for managing fleets of agents start to mature, each agent needs a clear role, scoped tools, and a way to improve over time. MEMBER.md defines that.

```
team/content-lead/
  MEMBER.md        # role, persona, permissions
  skills/          # planner, writer, copy-editor
  tools/           # x-search API
  journal/         # what it learned over time
```

## The problem

Agents start from scratch every session. The same setup, the same context, the same corrections. There's no way for an agent to develop a feel for the task or build on what it learned last time.

There are standards for individual pieces (SKILL.md for capabilities, MCP for tool servers, AGENTS.md for project instructions) but nothing for the complete worker. MEMBER.md bundles them into a single portable unit.

## Three parts

**Journal** - a daily log that traces what the agent did, what worked, and what the user preferred. The agent reads past entries before starting work. Over time it stops being a blank slate and builds real context.

**Skills** - a focused subset of capabilities the member owns. Each skill is a [SKILL.md](https://agentskills.io) file. The member gets better at these through use and journal feedback.

**Tools** - a scoped set of APIs, CLIs, and integrations the member has access to. Clear boundaries on what it can and can't touch.

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

Below the frontmatter: persona, workflow, and rules the member follows. Add `skills/`, `tools/`, `context/`, and `journal/` subdirectories as needed.

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
5. Read journal from `team/<name>/journal/` before starting
6. Operate within declared permissions
```

Activate directly:

```
You are the **content-lead**. Read `team/content-lead/MEMBER.md` before doing anything.
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
3. Load skills, tools, context, and journal from subdirectories
4. Follow the persona and rules in the body

No special parser. It's directories and markdown all the way down.

## Works with OpenClaw

MEMBER.md is designed to be agent-first. For platforms with their own memory system (e.g., OpenClaw's `memory/` directory), the journal maps onto that system. Skills map to OpenClaw skills. The format is portable across platforms.

## License

MIT
