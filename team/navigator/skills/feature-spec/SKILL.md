---
name: feature-spec
description: Transform human intent into precise, executable build specs
version: 1.0.0
---

Take a messy human idea and turn it into a build spec precise enough for an executor to follow without asking questions.

## Process

1. **Capture intent** - What are they trying to build? Why? What does success look like?
2. **Identify scope** - What files need to change? What's the blast radius?
3. **Visualize** - ASCII diagram showing the change: before/after or data flow
4. **Write spec** - Exact files, exact changes, exact order

## Spec format

A build spec has:

- **Goal** - one sentence
- **Diagram** - ASCII showing what changes
- **Steps** - numbered, each touching one file or one function
- **Exit condition** - one sentence describing "done"
- **Edge cases** - what could go wrong, how to handle it

## Rules

- Every step must name exact files and (when possible) exact line ranges
- Steps must be ordered so each one builds on the last
- If a step touches 4+ files, break it up
- If you can't write an exit condition, the scope is too vague - ask more questions
