# MEMBER.md Specification

**Version:** 0.1.0 (draft)

## Overview

A **member** is a directory containing everything an AI agent needs to operate as a specific team role. The directory MUST contain a `MEMBER.md` file and MAY contain skills, tool configurations, and context documents.

## Directory Structure

```
<member-name>/
├── MEMBER.md              REQUIRED  Manifest + persona
├── skills/                OPTIONAL  Capability definitions
│   └── <skill-name>/
│       └── SKILL.md
├── .mcp.json              OPTIONAL  MCP server configurations
└── context/               OPTIONAL  Knowledge documents
    └── *.md
```

### MEMBER.md (required)

The manifest file. Contains YAML frontmatter followed by markdown content.

#### Frontmatter Schema

```yaml
---
# REQUIRED fields
name: string               # kebab-case identifier (e.g., "sdr", "support-l1")
role: string               # human-readable title (e.g., "Sales Development Rep")

# RECOMMENDED fields
description: string        # one-line summary of what this member does
version: string            # semver (e.g., "1.0.0")

# OPTIONAL fields
skills: string[]           # skill names this member uses
permissions: object        # key-value permission declarations
mcps: string[]             # MCP server names from .mcp.json
---
```

#### Frontmatter Fields

**name** (required): Unique identifier for this member within the team. Must be kebab-case. Used for directory naming, activation commands, and references.

**role** (required): Human-readable job title. Used in UIs, listings, and when the agent introduces itself.

**description** (optional): One-line summary. Should describe the member's primary function, not their personality.

**version** (optional): Semantic version. Increment when the member's capabilities change meaningfully.

**skills** (optional): List of skill names. Skills are resolved in order:
1. Local: `./skills/<name>/SKILL.md`
2. Project-level: `<project>/skills/<name>/SKILL.md`
3. External: resolved by the tool (e.g., ClawHub, registry)

**permissions** (optional): Key-value pairs declaring what this member can and cannot do. Permissions are declarations — enforcement depends on the tool. Common patterns:
```yaml
permissions:
  can-send: false           # boolean capability
  can-draft: true
  can-delete: false
  approval-required:        # list of actions requiring human approval
    - send
    - delete
    - publish
  max-spend: 100            # numeric limits
  scope: internal           # enum constraints
```

**mcps** (optional): List of MCP server names. These reference entries in the member's `.mcp.json` file.

#### Body Content

Below the frontmatter, the body is free-form markdown. It typically includes:

- **Persona**: How the member communicates, their tone, decision-making style
- **Workflow**: Step-by-step process the member follows
- **Rules**: Hard constraints on behavior
- **Examples**: Sample interactions or outputs

The body is loaded as context when the member is activated.

### skills/ (optional)

Directory containing SKILL.md files following the [SKILL.md standard](https://agentskills.io). Each skill is a subdirectory with a SKILL.md file inside.

Skills define individual capabilities. A member composes multiple skills into a role.

### .mcp.json (optional)

MCP server configuration following the [MCP standard](https://modelcontextprotocol.io). Scoped to this member — only these tool servers are available when this member is active.

### context/ (optional)

Directory of markdown files containing domain knowledge this member needs. No special format — just documents the agent should reference.

Context files are loaded when the member is activated. They provide the domain expertise that makes a generic agent into a specialized worker.

## Team Directory Convention

Members live in a `team/` directory at the project root:

```
project/
├── CLAUDE.md              ← project instructions
├── team/
│   ├── sdr/MEMBER.md
│   ├── support/MEMBER.md
│   └── ops/MEMBER.md
└── src/
```

Tools detect members by scanning `team/*/MEMBER.md`.

## Portability

A member directory MUST be self-contained enough to function when copied to another project. The only external dependencies are:
- Shared skills referenced by name (graceful degradation if unavailable)
- MCP servers that require external setup (credentials, API keys)

A member directory SHOULD work with only its local skills and context, even if shared skills are unavailable.

## Versioning

This specification follows semantic versioning. The current version is **0.1.0** (draft).
