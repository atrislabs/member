---
name: copy-editor
description: Detect and remove AI writing patterns
---

# Copy Editor

Find and fix signs of AI-generated text. Based on Wikipedia's AI Cleanup patterns.

## Patterns to Kill

**Em dashes.** Replace all with commas, periods, or colons. No exceptions.

**Rule of three.** Never list exactly three parallel items. Two or four, not three.

**Negative parallelisms.** "Not just X, it's Y" and "It's not about X, it's about Y." Cut these.

**AI vocabulary.** Additionally, crucial, delve, landscape, pivotal, testament, vibrant, showcase, foster, underscore, enhance, garner, interplay, tapestry, enduring, intricate.

**-ing tacked-on phrases.** "...ensuring that," "...highlighting the," "...showcasing how." Cut or restructure.

**Copula avoidance.** "Serves as" and "stands as" should just be "is."

**Vague attributions.** "Experts say," "industry reports suggest." Name the source or cut it.

**Staccato overcorrection.** Five short fragments in a row is its own AI tell. Let one sentence breathe before the short punch.

## Pre-publish Checklist

Run before approving any writing:

```
[ ] No em dashes
[ ] No rule of three
[ ] No negative parallelisms
[ ] No AI vocabulary
[ ] No -ing tacked-on phrases
[ ] No "serves as" / "stands as"
[ ] No vague attributions
[ ] Sentence lengths vary
[ ] No staccato overcorrection
[ ] At least one specific detail
[ ] Ending is specific, not generic
```

If any box fails, rewrite and run again.
