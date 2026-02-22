---
name: daily-briefing
description: Build a personalized morning briefing from calendar, tasks, and news
version: 1.0.0
---

Build a morning briefing that covers what the user needs to know for the next 24 hours. Adapt structure and content based on the user's journal/memory.

## Briefing Sections

### Calendar (always included)

- Next 24 hours of events
- Flag meetings that need prep (external attendees, first-time meetings, presentations)
- Flag scheduling problems (back-to-back with no buffer, conflicts, double-bookings)
- Note focus time gaps - blocks of 90+ minutes with nothing scheduled

### Tasks (include if available)

- Top 3 priorities for today
- Overdue items with how many days overdue
- Deadlines in the next 48 hours
- Don't list everything - only what needs attention

### News (include if journal shows interest)

- Only topics the user has engaged with before
- 2-3 items max, one sentence each with source
- If no topic preferences exist yet, skip this section entirely - don't guess

### Prep Notes (include when meetings warrant it)

- For meetings the journal says the user cares about: who's attending, what was discussed last time, open questions
- For first-time meetings: who the person is, one line of context
- Don't prep routine standups or recurring 1:1s unless the journal says otherwise

## Format

Default format is tight bullets:

```
## Saturday Feb 22

**Changed:** Design review moved to 2pm (was 11am)

**Calendar**
- 10:00 - Sprint planning (prep: backlog is 23 items, need to cut to 10)
- 14:00 - Design review with Sarah (moved from 11am)
- 16:00-18:00 - Focus time (nothing scheduled)

**Tasks**
- Ship auth PR (3 days overdue)
- Review Q1 roadmap draft (due tomorrow)

**AI News**
- Anthropic launched Claude 4.5 Opus - code benchmarks up 15% (source)
```

Adapt based on journal preferences:
- Some users want prose paragraphs instead of bullets
- Some want just the calendar and nothing else
- Some want a priority-ranked single list mixing meetings and tasks
- Follow what the journal says. If no journal yet, use bullets.

## Anti-patterns

- Don't include weather unless the user has asked for it
- Don't include motivational quotes or greetings
- Don't summarize the whole calendar - only what needs attention
- Don't include news on topics the user hasn't shown interest in
- Don't pad with filler when it's a light day - "Nothing unusual today. Focus time available 10-12 and 2-5." is a valid briefing
