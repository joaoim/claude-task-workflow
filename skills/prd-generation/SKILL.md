---
name: prd-generation
description: >
  How to generate and review PRDs. Use when creating or reviewing a prd.md file.
  Includes the question framework, quality bar, and completeness checklist.
---

# PRD Generation Guide

## Question Framework

Before writing the PRD, the agent MUST ask the developer questions in these categories.
Do NOT proceed until answers are confirmed.

### Category 1: Functional Requirements
- What exactly should happen from the user's perspective?
- What are the key user flows?
- What inputs and outputs are expected?

### Category 2: Edge Cases & Error Handling
- What happens when input is invalid?
- What happens under failure conditions (network, timeout, partial data)?
- What are the boundary conditions?

### Category 3: Non-Functional Requirements
- What are the performance expectations (latency, throughput)?
- What are the security requirements?
- What scale does this need to handle?
- Are there observability requirements (logging, metrics, alerts)?

### Category 4: Scope Boundaries
- What are we explicitly NOT doing?
- Are there adjacent features that should be deferred?
- What is the minimum viable implementation?

### Category 5: Data & State
- Does this change the data model?
- Are there migrations required?
- How does this interact with existing state?

### Category 6: Dependencies & Constraints
- What other systems/services are involved?
- Are there API contracts to honor?
- What technical constraints exist?

## Quality Bar

A PRD is ready for review when:
- [ ] Every functional requirement has acceptance criteria (Given/When/Then)
- [ ] Every requirement has edge cases documented
- [ ] Non-functional requirements are measurable (not "fast" but "<200ms at p95")
- [ ] Scope section explicitly states what is out
- [ ] All unresolved questions are linked to proposed ADRs
- [ ] No ambiguous language ("should", "might", "probably", "could")
- [ ] Testing strategy covers unit, integration, and edge cases

## PRD Review Checklist

See `prd-checklist.md` for the full review checklist.
