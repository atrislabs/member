---
name: pattern-learning
description: Learn user preferences from briefing interactions and improve over time
version: 1.0.0
---

After every briefing, observe what happened and update the journal. This is how the chief of staff gets better.

## After Each Briefing

Write a journal entry to `journal/YYYY-MM-DD.md`:

```markdown
## Briefing - Feb 22

**Delivered:** calendar (5 events), tasks (3 items), news (2 AI items)
**Skipped:** weather, general news
**Follow-ups:** User asked about the 2pm design review attendees
**Signal:** User engaged with AI news, ignored task section
**Adjustment:** Next time, lead with news, move tasks below calendar
```

Keep entries short. Facts only - what was delivered, what the user did with it.

## Pattern Detection

After 3+ days of consistent behavior, write a durable preference to `journal/preferences.md`:

```markdown
## Briefing Preferences

- Skips weather (confirmed 5 consecutive days)
- Engages with AI/tech news (asked follow-ups 4 of 5 days)
- Prefers bullets over prose
- Wants prep notes for external meetings but not internal standups
- Monday: needs sprint planning context
- Friday: asks for weekly summary
```

Rules for promoting to durable memory:
- 3 consecutive days of the same behavior = write the preference
- A direct request ("never include weather") = write immediately, no waiting
- Contradicted behavior (user suddenly engages with something they usually skip) = note it but don't overwrite the preference until it happens 3 more times

## What to Track

| Signal | What it means | How to use it |
|--------|--------------|---------------|
| User asks follow-up | They care about this topic/meeting | Include more detail next time |
| User ignores a section | They don't need it daily | Reduce frequency or remove |
| User asks for something missing | Gap in the briefing | Add to tomorrow's briefing |
| User corrects the format | Wrong format preference | Update immediately |
| Day-of-week patterns | Routine varies by day | Build day-specific templates |
| User edits briefing time | Timing preference | Note preferred delivery window |

## Storage

Write to the member's `journal/` directory:

- **`journal/YYYY-MM-DD.md`** - Daily entries (append-only, one per briefing)
- **`journal/preferences.md`** - Durable preferences (updated when patterns are confirmed)

Before each briefing, read recent journal entries and `journal/preferences.md` to recall past patterns.

For platforms with their own memory system (e.g., OpenClaw's `memory/` directory and `memory_search`), map the journal onto that system instead.
