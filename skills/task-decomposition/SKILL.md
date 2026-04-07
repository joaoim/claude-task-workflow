---
name: task-decomposition
description: >
  How to decompose a PRD into implementation tasks.
  Use when creating tasks.md from an approved PRD.
---

# Task Decomposition Guide

## Rules

1. **Atomic:** Each task is one logical unit of work. If it touches more than
   2-3 files, consider splitting it.
2. **Testable:** Each task has acceptance criteria that can be verified with
   automated tests.
3. **Sequential numbering:** task-1, task-2, task-3, etc.
4. **Dependencies explicit:** If task-3 depends on task-1, say so.
5. **PRD traceability:** Every task references one or more PRD functional requirements.
6. **Parallelism:** Identify tasks with no shared dependencies — these can run in parallel.

## Task Sizing Guide

- **Too big:** "Implement the entire authentication flow" → split into:
  register endpoint, login endpoint, token refresh, middleware, tests.
- **Too small:** "Add import statement" → merge into the task that needs it.
- **Right size:** "Create the login endpoint with input validation,
  error handling, and unit tests."

## Dependency Graph

After listing all tasks, produce a dependency graph showing:
- Which tasks have no dependencies (can start immediately)
- Which tasks depend on others (must wait)
- Which groups can run in parallel

## Acceptance Criteria Format

Each criterion must be a checkbox that an agent can verify:
- [ ] Endpoint returns 200 with valid payload
- [ ] Endpoint returns 400 with missing required fields
- [ ] Unit test covers happy path and 2 error cases
- [ ] No regression in existing test suite
