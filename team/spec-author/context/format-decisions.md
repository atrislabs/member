# Format Decisions

Decisions made about the MEMBER.md format. Read this before proposing spec changes.

## Skills can be abstract references

Skills listed in frontmatter don't require a local SKILL.md. A member can declare capabilities without bundling the skill definition. This keeps flat-file members useful and acknowledges that skills may live in shared repositories.

## Two formats: flat file and directory

Flat files (`team/name.md`) for simple members. Directories (`team/name/MEMBER.md`) when a member bundles skills, tools, or context. Same frontmatter, same body format. Detection: directory first, flat file fallback.

## Only two required fields

Only `name` and `role` are required. Everything else is optional. This keeps the barrier to entry low - a member is just a name, a role, and some instructions.

## Permissions are conventions, not enforced

The spec defines permission conventions (`can-*`, `approval-required`) but doesn't enforce them. Enforcement is the responsibility of the tool or orchestrator. The spec provides a place to declare intent.

## No opinion on orchestration

MEMBER.md defines individual members. It doesn't define how members coordinate, who routes work, or how decisions are shared. Those are orchestration concerns that belong to the project, not the format.
