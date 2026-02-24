# MEMBER.md

A directory standard for defining AI agent team members.

```
team/content-lead/
  MEMBER.md        # role, persona, permissions
  skills/          # planner, writer, copy-editor
  tools/           # x-search API
  context/         # domain knowledge
  journal/         # what it learned over time
```

## Why

Agents start from scratch every session. No memory of what worked, no sense of what you prefer, no way to build on past runs. Meanwhile, standards exist for the pieces (SKILL.md, MCP, AGENTS.md) but not for the complete worker.

MEMBER.md bundles persona, skills, tools, context, and a journal into one portable directory.

## What's in a member

`journal/` tracks what the agent did and what the user preferred. The agent reads past entries before starting. This is what makes a member stateful.

`skills/` contains [SKILL.md](https://agentskills.io) files for the capabilities this member owns.

`tools/` documents the APIs, CLIs, and integrations this member can use.

`context/` holds domain knowledge: playbooks, SOPs, reference docs.

See [SPEC.md](./SPEC.md) for the full specification.

## Quick start

```yaml
---
name: sdr
role: Sales Development Rep
---
```

Two required fields. Everything else is optional. Add `skills/`, `tools/`, `context/`, and `journal/` when you need them. A flat file (`team/sdr.md`) works for simple members.

Use the [template](./template/MEMBER.md) to get started.

## Examples

Start with [content-lead/](./team/content-lead/), which has skills, tools, and a journal with real entries. Then look at [sdr/](./team/sdr/) for a full member with context docs.

| Member | Format | Description |
|--------|--------|-------------|
| [content-lead/](./team/content-lead/) | directory | Writing pipeline with journal that tracks voice preferences |
| [sdr/](./team/sdr/) | directory | Sales member with skills, tools, and context |
| [chief-of-staff/](./team/chief-of-staff/) | directory | Stateful briefing agent with journal loop |
| [navigator/](./team/navigator/) | directory | Planner with local skills |
| [executor.md](./team/executor.md) | flat file | Builder with abstract skill references |
| [validator.md](./team/validator.md) | flat file | Reviewer with approval gates |

## Usage

MEMBER.md is markdown. Point your AI tool at it.

```
You are the content-lead. Read team/content-lead/MEMBER.md before doing anything.
```

For multi-agent setups, each subagent reads a different member:

```
├── Task: "Act as navigator, read team/navigator/MEMBER.md, plan this feature"
├── Task: "Act as executor, read team/executor.md, build from the plan"
└── Task: "Act as validator, read team/validator.md, review the changes"
```

Works with Claude Code (CLAUDE.md), Codex (AGENTS.md), Cursor (.cursorrules), OpenClaw, or anything that reads markdown.

## License

MIT
