# Task Rules

## Ordering Principles

1. **Foundation first:** Tasks that establish shared infrastructure (types,
   schemas, config) come before tasks that use them.
2. **Data before logic:** Database changes and data models before business logic.
3. **Logic before UI:** Backend/core logic before presentation layer.
4. **Happy path before edge cases:** Core functionality before error handling
   (unless error handling is the core functionality).

## Dependency Declaration

- Use `Dependencies: none` for tasks that can start immediately.
- Use `Dependencies: task-1, task-3` for tasks that require prior work.
- Never create circular dependencies.
- If two tasks seem to depend on each other, one of them needs to be split.

## Parallel Execution Groups

After defining all tasks, group them by execution wave:

- **Wave 1:** All tasks with no dependencies (run in parallel, max 4).
- **Wave 2:** Tasks whose dependencies are all in Wave 1.
- **Wave N:** Tasks whose dependencies are all in prior waves.

## Task Completion Criteria

A task is only `done` when:
1. All acceptance criteria checkboxes are checked.
2. All tests pass (including existing test suite — no regressions).
3. The review-agent has verified all issues (or found none).

## Anti-Patterns

- **God task:** One task that does everything. Split it.
- **Orphan task:** A task with no PRD reference. Either link it or remove it.
- **Phantom dependency:** Declaring a dependency that doesn't actually exist.
  Only declare dependencies when task B literally cannot start without task A's output.
- **Test-last task:** Tests are part of the task, not a separate task.
