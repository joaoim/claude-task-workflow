---
name: ticket-workflow
description: >
  Core workflow knowledge for Jira-driven, ticket-scoped development.
  Use when working with /kickoff, /implement, or any ticket directory.
  Provides templates, status flows, naming conventions, and file layout rules.
---

# Ticket-Scoped Development Workflow

## Overview

Each Jira ticket gets its own directory with structured phases:
overview → PRD → PRD review → tasks → implementation → review.

## Directory Layout

{TICKET-ID}/
├── overview/overview.md
├── prd/prd.md
├── adr/adr-NNN.md
├── tasks/tasks.md
├── review/
│   ├── prd/issue-NNN.md
│   └── task-N/issue-NNN.md
└── memory/{role}-agent.md

## Phase Flow

1. **Overview** — Jira ticket + dev context → overview.md
2. **PRD** — Interactive Q&A → prd.md (status: draft → in-review → approved)
3. **PRD Review** — Review loop until all issues verified
4. **Tasks** — PRD → tasks.md (sequential, small, testable)
5. **Implementation** — Per-task: impl-agent writes code + tests
6. **Review** — Per-task: review-agent creates issues, impl-agent fixes, loop until verified

## Status Flows

### PRD: draft → in-review → approved
### Task: todo → in-progress → in-review → done
### Issue: open → fixed → verified
### ADR: proposed → accepted → superseded | deprecated

## Rules

- No agent guesses at business decisions. Unresolved questions become proposed ADRs.
- Each agent writes its own memory file after completing work.
- Tasks must be small enough for one agent session and independently testable.
- The orchestrator NEVER writes code. It only coordinates.
- Maximum 4 parallel agents to control token cost.

## Templates

Templates are available in this skill's `templates/` directory.
Read the appropriate template before creating any document.
