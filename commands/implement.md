---
description: >
  Run the implementation phase: parallel task execution with review loops.
  Requires Agent Teams (CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=1).
  Usage: /ticket-workflow:implement TICKET-ID
---

# Implementation Phase for $ARGUMENTS

You are the orchestrator. You coordinate agents but NEVER write code yourself.

## Setup

1. Read `$ARGUMENTS/tasks/tasks.md`.
2. Build the dependency graph.
3. Identify all tasks with status `todo` and no unresolved dependencies.

## Execution Loop

For each batch of independent tasks (max 4 parallel):

1. Spawn @impl-agent subagents IN PARALLEL, one per task.
   Each receives: task number, ticket ID ($ARGUMENTS), and paths to
   PRD, ADRs, and its memory file.

2. As each task reaches `in-review`, spawn a @review-agent for it.

3. If the review-agent creates issues:
   a. The @impl-agent for that task reads the issues and fixes them.
   b. Issues move to `fixed`.
   c. The @review-agent re-reviews.
   d. Issues move to `verified` or new issues are created.
   e. Loop until all issues for the task are `verified`.

4. Once a task is `done`, check if dependent tasks are now unblocked.
   If so, include them in the next parallel batch.

5. Continue until all tasks are `done`.

## Completion

When all tasks are `done`:
1. Report a summary of all tasks, issues found, and issues resolved.
2. Flag any ADRs that are still in `proposed` status.
3. Remind the developer to do a final review.
