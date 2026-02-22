---
name: codebase-scout
description: Explore and map a codebase to understand its structure
version: 1.0.0
---

Explore an unfamiliar codebase and produce a concise map of what exists, where things live, and how they connect.

## Process

1. **Entry points** - Find README, main config, package manifest. Read them.
2. **Directory scan** - Map the top-level structure. Note conventions.
3. **Key files** - Identify the 5-10 files that matter most (core logic, main entry, config, types/interfaces).
4. **Connections** - How do the key files relate? What imports what?
5. **Report** - 2-3 sentence summary, then an ASCII tree of what matters.

## Output format

Start with a summary: what this codebase does, what language/framework, how it's organized. Then show the tree with annotations:

```
src/
  core/         -- business logic
  api/          -- REST endpoints
  db/           -- models and migrations
tests/          -- mirrors src/ structure
config/         -- environment configs
```

## Anti-patterns

- Don't read every file. Scan structure first, then drill into key files.
- Don't produce a raw directory listing. Annotate and filter.
- Don't guess about what files do. Read them or say "unclear."
