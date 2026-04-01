# MEMBER.md Specification

Version 0.2.0

## Overview

MEMBER.md is a directory convention for defining intelligent team members. It composes existing standards -- [SKILL.md](https://agentskills.io) for capabilities, [AGENTS.md](https://github.com/anthropics/agents-md) for project instructions -- and any tool access (APIs, CLIs, MCP servers) into a single portable unit.

A member is a directory (or a single file) containing everything needed to operate a role: identity, skills, tools, permissions, and domain knowledge. The spec is operator-agnostic -- the same member definition works whether operated by a human, an AI, or both.

## Formats

### Flat file

```
team/<name>.md
```

A single markdown file with YAML frontmatter and instructions. Use this for simple members that don't need local skills, tools, or context files.

### Directory

```
team/<name>/MEMBER.md
```

A directory containing `MEMBER.md` plus optional subdirectories for skills, tools, and context. Use this when a member bundles its own capabilities or knowledge.

### Detection order

When resolving a member by name:

1. `team/<name>/MEMBER.md` (directory format)
2. `team/<name>.md` (flat file fallback)

Both formats use the same frontmatter and body structure.

## Frontmatter

YAML frontmatter at the top of the file, delimited by `---`.

### Required fields

| Field | Type | Description |
|-------|------|-------------|
| `name` | string | Kebab-case identifier. Must match the filename or directory name. |
| `role` | string | Human-readable job title. |

### Optional fields

| Field | Type | Description |
|-------|------|-------------|
| `description` | string | One-line summary of what this member does. |
| `version` | string | Semver version of this member definition. |
| `skills` | list | Capability names. See [Skills resolution](#skills-resolution). |
| `permissions` | map | Permission declarations. See [Permissions](#permissions). |
| `tools` | list | Tool names. See [Tools](#tools). |
| `agent-id` | string | Links this member to a cloud agent. Set by the platform on first push -- not manually authored. |

## Logs

Members maintain daily logs that make them stateful across sessions. Logs follow the Atris log convention:

```
<name>/
  logs/
    YYYY/
      YYYY-MM-DD.md
```

### Log format

Each daily log uses these sections:

```markdown
# Log -- YYYY-MM-DD

## Handoff
<!-- Key context for the next session to pick up -->

## Completed ✅
<!-- Work finished today -->

## In Progress 🔄
<!-- Active work -->

## Backlog
<!-- Queued work -->

## Notes
<!-- Observations, decisions, learnings -->

## Inbox
<!-- Incoming items to triage -->
```

Logs are append-only during the day. The Handoff section is the most important -- it's what the next session reads first to resume context. Durable preferences confirmed across multiple sessions go in `preferences.md` alongside the logs directory.

## Body

Markdown below the frontmatter defines the member's persona and operating instructions. The format is freeform, but the following sections are conventional:

- **Persona** - Who this member is. Tone, style, strengths, opinions.
- **Workflow** - Numbered steps for how the member operates.
- **Rules** - Hard constraints this member always follows.

## Atris Integration

MEMBER.md is native to the Atris operating system. Every member participates in the atris loop:

- **Logs** follow the atris log convention (Handoff, Completed, In Progress, Backlog, Notes, Inbox). See [Logs](#logs).
- **Goals** are checked every session. Skills like /flow read goals.md, track progress against it, and update it as priorities shift.
- **MAP.md** -- members know the workspace index. They don't grep blindly. They read MAP.md first.
- **Skills** -- members compose skills. A member with `skills: [flow, brain]` can invoke those capabilities.
- **Team** -- members can read other members' logs and delegate work across the team.
- **Plan-Do-Review** -- work flows through stages. Members respect the loop: understand before you act, validate after you build.

When a member is loaded (by /flow, by another skill, or by direct invocation), the loader should:

1. Read MEMBER.md (identity)
2. Read goals.md (ambition) -- create if missing
3. Read latest log (state)
4. Read MAP.md (navigation)

This gives the member full situational awareness from the first message.

## Directory structure

For directory-format members:

```
<name>/
  MEMBER.md              REQUIRED   Manifest and persona
  goals.md               AUTO       Created/updated by the member or by skills like /flow
  skills/                OPTIONAL   SKILL.md files (capabilities)
    <skill-name>/
      SKILL.md
  tools/                 OPTIONAL   Tool configurations
    *.md                            API references, CLI docs
    .mcp.json                       MCP servers (if needed)
  context/               OPTIONAL   Domain knowledge
    *.md
  logs/                  OPTIONAL   Daily logs (stateful across sessions)
    YYYY/
      YYYY-MM-DD.md
```

### goals.md

What this member is trying to achieve. Not a todo list -- those go in logs. Goals are directional. They answer "what does success look like this week/month/quarter?"

Goals are auto-managed. If goals.md doesn't exist, the first session that knows about goals (like /flow) creates it by asking the member what they're working toward. If it exists, skills read it to stay aligned and update it as priorities shift.

MEMBER.md enforces this: when reading a member, always check for `goals.md`. If missing, create it during the session. If stale (goals achieved or irrelevant), prompt to update.

Format is freeform markdown. Keep it short. A goal that takes a paragraph to explain is too vague.

### skills/

[SKILL.md](https://agentskills.io) files defining individual capabilities. Each skill lives in its own subdirectory. A member composes multiple skills into a role.

### tools/

Any tool access the member needs. REST APIs as markdown docs with endpoints and auth patterns, CLI tools as markdown docs with commands, MCP servers via `.mcp.json`. Most tools are just APIs -- document them in markdown and the agent can use them.

### context/

Markdown files with domain knowledge. Playbooks, ICPs, SOPs, reference docs. No special format -- just documents the member references while working.

### logs/

Daily logs that make a member stateful across sessions. See [Logs](#logs) for the full format specification.

The logs directory is empty on first install. It grows as the member works. For platforms with their own memory system (e.g., OpenClaw's `memory/` directory), the logs map onto that system instead.

## Skills resolution

When a skill name appears in the frontmatter `skills` list:

1. Check `./skills/<name>/SKILL.md` -- if found, this is a local skill with a full definition.
2. Otherwise, treat it as an abstract capability reference. The member has this capability but no local SKILL.md defines it.

Abstract skill references are useful for flat-file members or when the skill definition lives in a shared repository.

## Permissions

The `permissions` map declares what a member is and isn't allowed to do. The spec defines conventions but does not enforce them -- enforcement is the responsibility of the orchestrator or tool.

### Conventions

- `can-*` (boolean) - Capability flags. Examples: `can-read`, `can-execute`, `can-approve`, `can-send`.
- `approval-required` (list) - Actions that need human approval before proceeding. Examples: `[send, delete, refactor-outside-scope]`.

Permissions are declarations of intent. They tell the orchestrator (human or automated) what this member should and shouldn't do.

## Tools

The `tools` list names the external tools a member needs. For directory-format members, tool configurations live in the `tools/` subdirectory:

- Markdown files documenting APIs, CLIs, webhooks, or any integration
- `.mcp.json` for MCP server configurations (when applicable)

Tools are just things the member calls. An API documented in a markdown file is a tool. A CLI with a usage guide is a tool. MCP is one option, not the default.

For flat-file members, tool names serve as references to tools configured elsewhere.

## Principles

- **Compose, don't compete.** Uses existing standards. Doesn't reinvent them.
- **A directory, not a format.** The spec is a folder convention. No new parsers needed.
- **Portable by default.** Zip a member directory. It works anywhere.
- **Tool-agnostic.** Nothing is tied to a specific AI tool or vendor.
- **Operator-agnostic.** A member defines a role, not a species. Who operates it -- human, AI, or both -- is a runtime decision, not a spec concern.
