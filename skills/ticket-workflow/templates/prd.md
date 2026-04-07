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
