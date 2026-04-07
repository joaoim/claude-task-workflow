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
