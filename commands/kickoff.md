---
description: >
  Start the full ticket workflow: scaffold directories, create overview,
  generate PRD, review PRD, and decompose into tasks.
  Usage: /ticket-workflow:kickoff TICKET-ID
---

# Kickoff Workflow for $ARGUMENTS

You are the orchestrator. You coordinate agents but NEVER write code or
documents yourself. All work is delegated to specialized agents.

## Phase 1: Scaffold

Create the directory structure:
```
$ARGUMENTS/
├── overview/
├── prd/
├── adr/
├── tasks/
├── review/
└── memory/
```

## Phase 2: Overview

Use the @overview-agent to create the overview document.
Pass it the ticket ID: $ARGUMENTS.
Wait for the developer to review and approve the overview.

## Phase 3: PRD Generation

Use the @prd-agent to generate the PRD.
Pass it the ticket ID: $ARGUMENTS.
The agent will ask the developer questions interactively.
Wait for the developer to confirm the PRD is ready for review.

## Phase 4: PRD Review

Use the @prd-review-agent to review the PRD.
If issues are found:
  1. Show the issues to the developer.
  2. Use the @prd-agent to address the issues (read issue files, update PRD).
  3. @prd-agent marks issues as `fixed`.
  4. Use the @prd-review-agent to re-review.
  5. Loop until all issues are `verified`.
Update PRD status to `approved`.

## Phase 5: Task Decomposition

Use the @task-agent to break down the PRD.
Show the task list and dependency graph to the developer for approval.

## Gate

Stop here. Report the task list and ask the developer to review.
Implementation requires running `/ticket-workflow:implement $ARGUMENTS`.
