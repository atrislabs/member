# MEMBER.md Specification

Version 0.1.0

## Overview

MEMBER.md is a directory convention for defining complete AI team members. It composes existing standards -- [SKILL.md](https://agentskills.io) for capabilities, [AGENTS.md](https://github.com/anthropics/agents-md) for project instructions -- and any tool access (APIs, CLIs, MCP servers) into a single portable unit.

A member is a directory (or a single file) containing everything an AI agent needs to assume a role: identity, skills, tools, permissions, and domain knowledge.

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

## Body

Markdown below the frontmatter defines the member's persona and operating instructions. The format is freeform, but the following sections are conventional:

- **Persona** - Who this member is. Tone, style, strengths, opinions.
- **Workflow** - Numbered steps for how the member operates.
- **Rules** - Hard constraints this member always follows.

## Directory structure

For directory-format members:

```
<name>/
  MEMBER.md              REQUIRED   Manifest and persona
  skills/                OPTIONAL   SKILL.md files (capabilities)
    <skill-name>/
      SKILL.md
  tools/                 OPTIONAL   Tool configurations
    *.md                            API references, CLI docs
    .mcp.json                       MCP servers (if needed)
  context/               OPTIONAL   Domain knowledge
    *.md
  journal/               OPTIONAL   Accumulated learning
    *.md
```

### skills/

[SKILL.md](https://agentskills.io) files defining individual capabilities. Each skill lives in its own subdirectory. A member composes multiple skills into a role.

### tools/

Any tool access the member needs. REST APIs as markdown docs with endpoints and auth patterns, CLI tools as markdown docs with commands, MCP servers via `.mcp.json`. Most tools are just APIs -- document them in markdown and the agent can use them.

### context/

Markdown files with domain knowledge. Playbooks, ICPs, SOPs, reference docs. No special format -- just documents the member references while working.

### journal/

Accumulated learning from past runs. The journal is what makes a member stateful -- it reads past entries before working and writes new entries after. Over time, the member adapts to the user's preferences and patterns without being explicitly configured.

Convention:
- Daily entries in `YYYY-MM-DD.md` (append-only run logs)
- Durable preferences in `preferences.md` (updated when patterns are confirmed across multiple runs)

The journal directory is empty on first install. It grows as the member works. For platforms with their own memory system (e.g., OpenClaw's `memory/` directory), the journal maps onto that system instead.

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
