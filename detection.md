# Detection

How AI tools detect and activate MEMBER.md team members. No platform changes required — this works today with convention-based detection.

## Core Principle

MEMBER.md is markdown. Every AI coding tool already reads markdown. Detection is just telling the tool where to look.

## Claude Code

### Detection via CLAUDE.md

Add to your project's CLAUDE.md:

```markdown
## Team

This project uses MEMBER.md team members. Available members are in `team/`.

When activated as a specific member:
1. Read `team/<name>/MEMBER.md` for persona, role, and permissions
2. Load skills from `team/<name>/skills/`
3. Load context from `team/<name>/context/`
4. Use tool servers from `team/<name>/.mcp.json`
5. Operate within declared permissions
```

That's it. Claude Code reads CLAUDE.md on every session. It now knows about your team.

### Activation patterns

**Direct activation (in CLAUDE.md):**
```markdown
You are the **navigator**. Read `team/navigator/MEMBER.md` before doing anything.
```

**Subagent activation (Task tool):**
```
"Act as the validator. Read team/validator/MEMBER.md and follow its instructions.
Review the changes in the last 3 commits."
```

**User-triggered:**
```
User: "be the sdr"
Agent: *reads team/sdr/MEMBER.md, loads skills and context*
```

### Multi-agent via Task tool

Claude Code already spawns subagents. Each subagent can be a different member:

```
Main agent (reads CLAUDE.md)
├── Task: "Act as navigator — read team/navigator/MEMBER.md, plan this feature"
├── Task: "Act as executor — read team/executor/MEMBER.md, build from the plan"
└── Task: "Act as validator — read team/validator/MEMBER.md, review the changes"
```

Each subagent loads a different MEMBER.md. Different persona, different skills, different permissions. Multi-agent team from one project directory.

## Codex (OpenAI)

### Detection via AGENTS.md

```markdown
## Team Members

This project defines AI team members in `team/*/MEMBER.md`.
Each member has scoped skills, tools, and context.
When assigned a role, read the corresponding MEMBER.md first.
```

Codex reads AGENTS.md. Same convention, different file.

### Codex task assignment

```
codex "As the support agent (see team/support/MEMBER.md), triage these new issues"
```

## Cursor

### Detection via .cursorrules

```markdown
This project has AI team members defined in team/*/MEMBER.md.
When working on support tasks, reference team/support/MEMBER.md.
When working on SDR tasks, reference team/sdr/MEMBER.md.
```

### Cursor agent mode

Cursor's agent mode reads referenced files. Point it at a MEMBER.md and it loads the context.

## Generic detection (any tool)

Any AI tool that reads markdown can detect members. The convention is:

1. Scan for `team/*/MEMBER.md` in the project root
2. Parse YAML frontmatter for member metadata
3. Resolve skills from `team/<name>/skills/`
4. Load `.mcp.json` from member directory
5. Read `context/*.md` files

No special parser needed. It's directories and markdown all the way down.

## CLI activation (Atris)

For tools that support it, a CLI command can automate activation:

```bash
# List available members
atris member list

# Activate a member (symlinks skills, loads context)
atris member activate navigator

# Scaffold a new member
atris member create sdr
```

The CLI is convenience, not requirement. The format works without any CLI — it's just files.
