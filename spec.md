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
├── tools/                 OPTIONAL  Tool configurations
│   ├── .mcp.json                    MCP servers
│   └── *.md                         API references, CLI tools, etc.
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
tools: string[]            # tool names (MCP servers, APIs, CLIs, etc.)
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

**tools** (optional): List of tool names this member has access to. Tools can be:
- MCP servers (configured in `tools/.mcp.json`)
- API integrations (documented in `tools/*.md` with endpoints and auth)
- CLI tools (documented in `tools/*.md` with commands)
- Any other tool access the member needs

Tools are broader than MCP. An SDR might have HubSpot via MCP server, Apollo via REST API with a bearer token, and LinkedIn via browser automation. All three go in `tools/`.

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

### tools/ (optional)

Directory containing tool configurations scoped to this member. Can include:

- **`.mcp.json`** — MCP server configurations following the [MCP standard](https://modelcontextprotocol.io)
- **`<tool-name>.md`** — API references, CLI docs, or any tool documentation the member needs. Follows the same pattern as Atris integration skills: endpoint, auth method, example requests.

Not all tools are MCP servers. REST APIs with bearer tokens, CLI tools, browser automation — anything the member uses to interact with external systems belongs here.

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
- Tools that require external setup (API keys, MCP servers, CLI installations)

A member directory SHOULD work with only its local skills and context, even if shared skills are unavailable.

## Versioning

This specification follows semantic versioning. The current version is **0.1.0** (draft).
