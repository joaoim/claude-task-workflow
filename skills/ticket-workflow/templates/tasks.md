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
