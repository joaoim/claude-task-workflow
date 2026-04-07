---
name: memory-management
description: >
  How agents should read and write memory files. Use at the start and end
  of any agent's work session.
---

# Agent Memory Management

## When to Read Memory

At the START of every work session, before doing anything else:
1. Check if your memory file exists at `memory/{your-agent-name}.md`.
2. If it exists, read it. The "Key Context" section is your minimum viable context.
3. If it doesn't exist, proceed without prior context.

## When to Write Memory

At the END of every work session, ALWAYS:
1. Create or update your memory file.
2. Capture: key context, decisions made, problems encountered, assumptions.
3. Write handoff notes for the next agent or your future self.

## What to Capture

- **Key Context:** The 3-5 most important things to remember. If you could only
  read one section, this is it.
- **Decisions Made:** Local implementation decisions and why. Not ADR-level.
- **Problems Encountered:** What went wrong and how you solved it.
- **Assumptions:** What you assumed to be true. Flag if later invalidated.
- **Work Log:** Chronological record of significant actions.
- **Handoff Notes:** What the next agent needs to know.

## File Naming

- `memory/overview-agent.md`
- `memory/prd-agent.md`
- `memory/prd-review-agent.md`
- `memory/task-{N}-impl-agent.md`
- `memory/task-{N}-review-agent.md`

## Size Limit

Keep memory files under 200 lines. If a file exceeds this, summarize older
entries and archive details to a separate file.
