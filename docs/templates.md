# Ticket-Scoped Development Workflow — Templates

---

## 1. overview.md

```markdown
---
ticket: {TICKET-ID}
title: "{Jira ticket title}"
created: {YYYY-MM-DD}
author: "{dev name}"
jira-url: "{link to Jira ticket}"
status: draft | ready
---

# Overview: {TICKET-ID}

## Jira Summary

{Paste or summarize the Jira ticket description here. Include the acceptance criteria as stated by the product team.}

## Business Context

{Why does this matter? What business goal or user problem does this address? What triggered this work now?}

## Developer Considerations

{Add any technical context, observations, or concerns that aren't in the Jira ticket but are important for understanding the work. This is where the dev's expertise shapes the direction.}

### Known Constraints

- {e.g., Must work within the existing auth system}
- {e.g., Cannot modify the public API contract}
- {e.g., Must be backwards compatible with v2.x}

### Known Risks

- {e.g., The payments service has no staging environment}
- {e.g., This touches a high-traffic path}

### Dependencies

- {e.g., Requires FOO-8 to be merged first}
- {e.g., Depends on the billing team's API being available}

### Initial Assumptions

- {e.g., We assume the existing DB schema can support the new field}
- {e.g., We assume the feature flag system is available}

## Open Questions

{List anything that needs clarification from business, product, or other teams. Each of these should become a proposed ADR if not resolved before PRD generation.}

- [ ] {Question 1}
- [ ] {Question 2}

## References

- {Link to relevant docs, Slack threads, design files, related tickets}
```

---

## 2. prd.md

```markdown
---
ticket: {TICKET-ID}
title: "{Feature/change title}"
version: 1.0
created: {YYYY-MM-DD}
last-updated: {YYYY-MM-DD}
author: prd-agent
reviewed-by: prd-review-agent
status: draft | in-review | approved
open-questions: {count}
adrs-linked: []
---

# PRD: {TICKET-ID} — {Feature/change title}

## 1. Problem Statement

{What problem are we solving? Describe it from the user's or system's perspective. Be specific — not "improve performance" but "reduce checkout latency for users on mobile connections from ~4s to under 1.5s."}

## 2. Goals & Success Criteria

{What does "done" look like? Use measurable criteria where possible.}

- **Goal 1:** {description}
  - Success metric: {how we measure it}
- **Goal 2:** {description}
  - Success metric: {how we measure it}

## 3. Scope

### In Scope

- {Specific capability or change 1}
- {Specific capability or change 2}

### Out of Scope

- {What we are explicitly NOT doing and why}
- {Adjacent work that is deferred}

## 4. Functional Requirements

{Describe what the system must do. Each requirement should be specific, testable, and traceable to a goal above.}

### FR-1: {Requirement title}

- **Description:** {What the system must do}
- **Acceptance criteria:**
  - {Given... When... Then...}
  - {Given... When... Then...}
- **Edge cases:**
  - {What happens when...}

### FR-2: {Requirement title}

- **Description:** {What the system must do}
- **Acceptance criteria:**
  - {Given... When... Then...}
- **Edge cases:**
  - {What happens when...}

## 5. Non-Functional Requirements

- **Performance:** {e.g., Response time < 200ms at p95}
- **Security:** {e.g., Input must be sanitized, auth required}
- **Scalability:** {e.g., Must handle 10x current load}
- **Accessibility:** {e.g., WCAG 2.1 AA compliance}
- **Observability:** {e.g., Structured logging, metrics, alerts}

## 6. Technical Constraints

- {e.g., Must use existing PostgreSQL instance}
- {e.g., No new external dependencies without ADR}
- {e.g., Must maintain backwards compatibility with API v2}

## 7. Dependencies

### Internal

- {Service/module this depends on}
- {Related ticket that must be completed first}

### External

- {Third-party API, team, or system}

## 8. Data & State Changes

{Describe any changes to data models, database schemas, state machines, or data flows. If none, state "No data changes required."}

- {New fields, tables, migrations}
- {Changes to existing data structures}
- {Data migration strategy if applicable}

## 9. Error Handling & Failure Modes

{How should the system behave when things go wrong?}

- **Scenario:** {What fails}
  - **Expected behavior:** {What the system does}
  - **User impact:** {What the user sees}

## 10. Security Considerations

- {Authentication/authorization requirements}
- {Data sensitivity and handling}
- {Input validation requirements}
- {Audit logging needs}

## 11. Testing Strategy

- **Unit tests:** {What should be unit tested}
- **Integration tests:** {What integrations need testing}
- **Edge case tests:** {Specific edge cases to cover}
- **Manual verification:** {Anything that can't be automated}

## 12. Open Questions & ADR References

{Questions that remain unresolved. Each should link to a proposed ADR.}

| # | Question | Status | ADR |
|---|----------|--------|-----|
| 1 | {Question text} | open / resolved | [ADR-001](../adr/adr-001.md) |
| 2 | {Question text} | open / resolved | — |

## 13. Assumptions

{Assumptions made during PRD creation that, if wrong, would change the approach. Each assumption is a risk.}

- {Assumption 1}
- {Assumption 2}

## 14. Revision History

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0 | {date} | prd-agent | Initial draft |
```

---

## 3. tasks.md

```markdown
---
ticket: {TICKET-ID}
created: {YYYY-MM-DD}
last-updated: {YYYY-MM-DD}
total-tasks: {count}
completed: {count}
status: pending | in-progress | completed
---

# Tasks: {TICKET-ID}

## Summary

{Brief description of what this task breakdown covers, referencing the PRD.}

Source: [PRD](../prd/prd.md)

---

## Task 1: {Short descriptive title}

- **Status:** todo | in-progress | in-review | done
- **PRD Reference:** FR-1
- **Description:** {What needs to be done. One clear, atomic unit of work.}
- **Acceptance Criteria:**
  - [ ] {Testable criterion 1}
  - [ ] {Testable criterion 2}
- **Test Expectations:**
  - {What tests must be written or pass}
- **Files Likely Affected:**
  - `{path/to/file}`
- **Dependencies:** none | task-{N}
- **Notes:** {Any implementation hints or things to watch out for}

---

## Task 2: {Short descriptive title}

- **Status:** todo
- **PRD Reference:** FR-1, FR-2
- **Description:** {What needs to be done.}
- **Acceptance Criteria:**
  - [ ] {Testable criterion 1}
  - [ ] {Testable criterion 2}
- **Test Expectations:**
  - {What tests must be written or pass}
- **Files Likely Affected:**
  - `{path/to/file}`
- **Dependencies:** task-1
- **Notes:** {Any implementation hints}

---

{Continue for each task...}

---

## Task Order & Dependency Graph

{Optional: a simple text-based dependency graph or ordered list showing which tasks can run in parallel vs. which are sequential.}

1. Task 1 (no dependencies)
2. Task 2 (depends on Task 1)
3. Task 3 (no dependencies — can run parallel with Task 1)
4. Task 4 (depends on Task 2, Task 3)
```

---

## 4. issue.md (Review Issue Template)

```markdown
---
ticket: {TICKET-ID}
task: task-{N}
issue: {NNN}
status: open | fixed | verified
severity: critical | major | minor | suggestion
found-by: {reviewer-agent-id}
fixed-by: {impl-agent-id}
verified-by: {reviewer-agent-id}
created: {YYYY-MM-DD}
last-updated: {YYYY-MM-DD}
---

# Issue {NNN}: {Short descriptive title}

## Category

{One of: bug | logic-error | missing-requirement | edge-case | security | performance | style | test-coverage | documentation}

## Description

{Clear explanation of what is wrong. Be specific — reference exact behavior, not vague concerns.}

## Location

- **File(s):** `{path/to/file}:{line-range}`
- **Function/Component:** `{name}`

## Expected Behavior

{What should happen according to the PRD or acceptance criteria.}

## Actual Behavior

{What actually happens or what the code currently does.}

## Suggested Fix

{How this should be resolved. Be specific enough that the implementation agent can act on it without guessing.}

## Verification Steps

{How the reviewer will confirm the fix is correct.}

- [ ] {Step 1}
- [ ] {Step 2}

## Related

- PRD Reference: {FR-N or NFR}
- Task: [task-{N}](../../tasks/tasks.md#task-{N})
- Related Issues: {links to other issues if connected}
```

---

## 5. adr.md (Architecture/Any Decision Record)

```markdown
---
ticket: {TICKET-ID}
adr: {NNN}
title: "{Decision title — short imperative phrase}"
status: proposed | accepted | superseded | deprecated
date: {YYYY-MM-DD}
decision-makers: ["{dev}", "{product-owner}"]
asked-by: {agent-id | dev}
answered-by: {product | business | tech-lead | dev}
superseded-by: {ADR-NNN | null}
---

# ADR-{NNN}: {Decision title}

## Context and Problem Statement

{What question or ambiguity came up, and why does it need a decision? Where in the workflow did it surface — during PRD generation, PRD review, task breakdown, or implementation? Link to the source.}

- Source: [prd.md#section](../prd/prd.md#section) | [task-N](../tasks/tasks.md#task-N) | [issue-NNN](../review/task-N/issue-NNN.md)

## Decision Drivers

- {Driver 1: e.g., consistency with existing system behavior}
- {Driver 2: e.g., business rule from product team}
- {Driver 3: e.g., technical constraint}

## Considered Options

### Option A: {Name}

{Brief description.}

- Good, because {pro}
- Good, because {pro}
- Bad, because {con}

### Option B: {Name}

{Brief description.}

- Good, because {pro}
- Bad, because {con}
- Bad, because {con}

## Decision Outcome

**Chosen option:** "{Option name}", because {primary justification}.

### Consequences

- Good: {positive consequence}
- Good: {positive consequence}
- Bad: {negative consequence or tradeoff accepted}

### Confirmation

{How will we verify this decision was implemented correctly? e.g., code review check, test, or manual verification.}

## More Information

{Any additional context, links to Slack threads, meeting notes, documentation, or related ADRs.}

- Related ADRs: [ADR-{NNN}](./adr-{NNN}.md)
```

---

## 6. memory.md (Agent Memory Template)

```markdown
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
```