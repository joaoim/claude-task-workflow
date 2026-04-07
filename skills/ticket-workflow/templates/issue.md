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
