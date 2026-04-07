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
