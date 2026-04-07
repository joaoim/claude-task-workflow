---
name: code-review
description: >
  Standards for reviewing implementation work. Use when reviewing code for a task.
  Includes severity definitions, what to check, and issue file format.
---

# Code Review Standards

## What to Check

1. **Correctness:** Does the code satisfy the task's acceptance criteria?
2. **Edge cases:** Are PRD-defined edge cases handled?
3. **Error handling:** Are failures handled gracefully?
4. **Security:** Input validation, auth checks, data sanitization.
5. **Performance:** No N+1 queries, unnecessary loops, or memory leaks.
6. **Test coverage:** Are tests present and do they cover acceptance criteria?
7. **Code quality:** Readability, naming, duplication, complexity.

## Severity Definitions

- **critical:** Broken functionality, security vulnerability, data loss risk.
  Must be fixed before task can be marked done.
- **major:** Logic error, missing edge case, inadequate test coverage.
  Should be fixed before task can be marked done.
- **minor:** Style issue, naming improvement, minor refactor opportunity.
  Can be fixed but won't block the task.
- **suggestion:** Nice-to-have improvement. Informational only.

## Issue File Rules

- One issue per file in `review/{task-N}/issue-NNN.md`.
- Issues numbered sequentially: issue-001, issue-002, etc.
- Each issue must include: description, location, expected vs actual behavior,
  suggested fix, and verification steps.
- Status flow: open → fixed → verified.
- The reviewer MUST re-run tests and re-check the code before marking verified.
- If a fix introduces new problems, create new issue files.
