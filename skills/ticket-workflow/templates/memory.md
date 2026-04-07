---
ticket: {TICKET-ID}
agent: {agent-role} (e.g., prd-agent, task-1-impl-agent, task-1-review-agent)
task: {task-N | prd | overview | global}
created: {YYYY-MM-DD}
last-updated: {YYYY-MM-DD}
---

# Memory: {agent-role} — {task reference}

## Agent Role & Scope

{One-line description of what this agent is responsible for in this context.}

## Key Context

{The most important things this agent needs to remember. If this agent is resumed or a new session starts, this section is the minimum viable context.}

- {Critical fact 1}
- {Critical fact 2}
- {Critical fact 3}

## Decisions Made

{Decisions this agent made during its work, and why. These are local implementation decisions — not ADR-level decisions.}

| Decision | Rationale | Timestamp |
|----------|-----------|-----------|
| {What was decided} | {Why} | {YYYY-MM-DD} |

## Problems Encountered

{Issues hit during work, whether resolved or not.}

| Problem | Resolution | Status |
|---------|------------|--------|
| {Description} | {How it was resolved or workaround} | resolved / open |

## Assumptions Made

{Things this agent assumed to be true. If any assumption is later invalidated, this is where to look.}

- {Assumption 1}
- {Assumption 2}

## Work Log

{Chronological record of significant actions taken.}

- [{YYYY-MM-DD}] {What was done}
- [{YYYY-MM-DD}] {What was done}

## Handoff Notes

{If this agent's work feeds into another agent's work, what should that next agent know? Think of this as a briefing for the next person.}

- {Key thing the next agent should know}
- {Gotcha or trap to avoid}
- {File or section to pay special attention to}

## References

- [PRD](../prd/prd.md)
- [Tasks](../tasks/tasks.md)
- [Related ADRs](../adr/)
- [Review Issues](../review/)
