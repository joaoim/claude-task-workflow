# PRD Review Checklist

## Completeness

- [ ] Problem statement is specific and measurable
- [ ] All goals have success metrics
- [ ] In-scope items are enumerated
- [ ] Out-of-scope items are explicitly listed with rationale
- [ ] Every functional requirement has acceptance criteria
- [ ] Every functional requirement has edge cases
- [ ] Non-functional requirements use measurable thresholds
- [ ] Technical constraints are documented
- [ ] Dependencies (internal and external) are listed
- [ ] Data and state changes are described
- [ ] Error handling and failure modes are covered
- [ ] Security considerations are addressed
- [ ] Testing strategy is defined

## Consistency

- [ ] No contradictions between sections
- [ ] Acceptance criteria align with goals
- [ ] Edge cases align with error handling section
- [ ] Dependencies match technical constraints
- [ ] Scope boundaries are consistent throughout

## Clarity

- [ ] No ambiguous language ("should", "might", "probably", "could")
- [ ] No vague quantities ("many", "few", "some", "fast", "large")
- [ ] Each requirement is independently understandable
- [ ] Acronyms and domain terms are defined or obvious from context

## Traceability

- [ ] Every requirement traces to a goal
- [ ] Every open question has an ADR or is marked resolved
- [ ] Assumptions are listed and flagged as risks
- [ ] Revision history is current

## Cross-Reference with Overview

- [ ] All overview concerns are addressed in the PRD
- [ ] Known constraints from overview appear in technical constraints
- [ ] Known risks from overview are reflected in error handling or assumptions
- [ ] Dependencies from overview are listed in the PRD
- [ ] Open questions from overview are resolved or have ADRs
