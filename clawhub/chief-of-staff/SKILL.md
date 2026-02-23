---
name: chief-of-staff
description: |
  Daily briefing agent that learns your patterns and gets better every morning.
  Requires: member-runtime skill.
metadata: { "openclaw": { "emoji": "📋", "requires": { "env": [] } } }
---

# Chief of Staff

A MEMBER.md worker that runs on the member-runtime skill. Install both:

```
clawhub install member-runtime
clawhub install chief-of-staff
```

Then say: "brief me" or "be my chief of staff"

## What This Member Does

Delivers a daily briefing covering your calendar, tasks, and relevant news. Gets better over time through a journal loop - tracks what you engage with, what you skip, and adapts.

- **Day 1:** Generic briefing. Calendar + tasks. Fine but forgettable.
- **Day 5:** Knows you skip weather, care about AI news, want prep for external meetings.
- **Day 30:** Knows your Monday sprint routine, your team, your reading habits. The briefing is yours.

## Files Installed

```
team/chief-of-staff/
  MEMBER.md                          # Persona and workflow
  skills/daily-briefing/SKILL.md     # How to build the briefing
  skills/pattern-learning/SKILL.md   # How to learn from each run
  context/preferences.md             # Starting defaults
```

## How It Works

The member-runtime skill loads these files automatically. The chief-of-staff persona takes over, follows its workflow, and writes a journal entry after each briefing. No configuration needed - preferences build up through use.

## Source

Format spec and full source: https://github.com/atrislabs/member
