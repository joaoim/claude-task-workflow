# Orchestration Blueprint — Plugin Architecture

## Why a Plugin

You have existing projects with their own CLAUDE.md. You don't want to
modify them. A plugin gives you:

- **Zero-touch installation** — no changes to existing CLAUDE.md
- **Portability** — install once, use in any project
- **Namespacing** — plugin skills/commands won't collide with existing ones
- **Versioning** — update the plugin, all projects get the update
- **Shareability** — your team installs it from a GitHub repo

## Plugin Structure

```
ticket-workflow-plugin/
├── .claude-plugin/
│   └── plugin.json              ← plugin manifest (required)
│
├── skills/
│   ├── ticket-workflow/
│   │   ├── SKILL.md             ← core workflow: phases, rules, status flows
│   │   └── templates/
│   │       ├── overview.md
│   │       ├── prd.md
│   │       ├── tasks.md
│   │       ├── issue.md
│   │       ├── adr.md
│   │       └── memory.md
│   │
│   ├── prd-generation/
│   │   ├── SKILL.md             ← PRD writing: questions, structure, quality bar
│   │   └── prd-checklist.md
│   │
│   ├── task-decomposition/
│   │   ├── SKILL.md             ← how to break PRDs into small testable tasks
│   │   └── task-rules.md
│   │
│   ├── code-review/
│   │   ├── SKILL.md             ← review standards, severity, checklist
│   │   └── review-checklist.md
│   │
│   └── memory-management/
│       ├── SKILL.md             ← how agents read/write memory files
│       └── memory-rules.md
│
├── agents/
│   ├── overview-agent.md
│   ├── prd-agent.md
│   ├── prd-review-agent.md
│   ├── task-agent.md
│   ├── impl-agent.md
│   └── review-agent.md
│
├── commands/
│   ├── kickoff.md               ← /ticket-workflow:kickoff FOO-10
│   ├── implement.md             ← /ticket-workflow:implement FOO-10
│   ├── status.md                ← /ticket-workflow:status FOO-10
│   └── adr-resolve.md           ← /ticket-workflow:adr-resolve FOO-10/adr/adr-001.md
│
└── README.md
```

**Key point:** Skills from plugins are namespaced automatically
(e.g., `ticket-workflow:prd-generation`). Commands become
`/ticket-workflow:kickoff`. Agents are available by name
(e.g., `@prd-agent`). Nothing collides with the project's
existing `.claude/` directory.

---

## Plugin Manifest

```json
// .claude-plugin/plugin.json
{
  "name": "ticket-workflow",
  "version": "1.0.0",
  "description": "Jira-driven, multi-agent development workflow. Scoped per ticket with structured phases: overview → PRD → tasks → implementation → review. Includes ADR tracking and per-agent memory.",
  "author": "your-github-username"
}
```

---

## Skills

### ticket-workflow/SKILL.md

```markdown
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
```

### prd-generation/SKILL.md

```markdown
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
```

### task-decomposition/SKILL.md

```markdown
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
```

### code-review/SKILL.md

```markdown
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
```

### memory-management/SKILL.md

```markdown
---
name: memory-management
description: >
  How agents should read and write memory files. Use at the start and end
  of any agent's work session.
---

# Agent Memory Management

## When to Read Memory

At the START of every work session, before doing anything else:
1. Check if your memory file exists at `memory/{your-agent-name}.md`.
2. If it exists, read it. The "Key Context" section is your minimum viable context.
3. If it doesn't exist, proceed without prior context.

## When to Write Memory

At the END of every work session, ALWAYS:
1. Create or update your memory file.
2. Capture: key context, decisions made, problems encountered, assumptions.
3. Write handoff notes for the next agent or your future self.

## What to Capture

- **Key Context:** The 3-5 most important things to remember. If you could only
  read one section, this is it.
- **Decisions Made:** Local implementation decisions and why. Not ADR-level.
- **Problems Encountered:** What went wrong and how you solved it.
- **Assumptions:** What you assumed to be true. Flag if later invalidated.
- **Work Log:** Chronological record of significant actions.
- **Handoff Notes:** What the next agent needs to know.

## File Naming

- `memory/overview-agent.md`
- `memory/prd-agent.md`
- `memory/prd-review-agent.md`
- `memory/task-{N}-impl-agent.md`
- `memory/task-{N}-review-agent.md`

## Size Limit

Keep memory files under 200 lines. If a file exceeds this, summarize older
entries and archive details to a separate file.
```

---

## Agents

### agents/overview-agent.md

```markdown
---
name: overview-agent
description: >
  Creates overview.md from a Jira ticket and developer input.
  Use when starting a new ticket workflow with /kickoff.
tools: Read, Write, Edit, Bash, Grep, Glob
skills: ticket-workflow, memory-management
model: sonnet
memory:
  enabled: true
  scope: project
---

You are the Overview Agent for the ticket-workflow plugin.

## Job

Create the initial overview document for a Jira ticket.

## Process

1. Read the Jira ticket details provided in your prompt.
2. Read the overview template from the ticket-workflow skill.
3. Ask the developer about:
   - Business context not captured in the ticket
   - Known technical constraints and risks
   - Dependencies on other work
   - Assumptions they're already making
4. Generate `{ticket-id}/overview/overview.md` from the template.
5. For any question the dev can't answer, note it in the Open Questions section.
6. Write your memory to `{ticket-id}/memory/overview-agent.md`.
```

### agents/prd-agent.md

```markdown
---
name: prd-agent
description: >
  Generates a detailed PRD through interactive Q&A with the developer.
  Use after overview.md is created. Invoked by /kickoff or directly.
tools: Read, Write, Edit, Bash, Grep, Glob
skills: ticket-workflow, prd-generation, memory-management
model: opus
memory:
  enabled: true
  scope: project
---

You are the PRD Agent for the ticket-workflow plugin.

## Job

Produce a precise, detailed PRD that eliminates ambiguity.

## Process

1. Read `{ticket-id}/overview/overview.md`.
2. Read any existing ADRs in `{ticket-id}/adr/`.
3. Read the PRD template and the prd-generation skill.
4. Ask the developer ALL clarifying questions organized by the 6 categories
   in the prd-generation skill. Do NOT proceed until answers are confirmed.
5. Generate `{ticket-id}/prd/prd.md`.
6. For any question the developer cannot answer (needs business input),
   create a proposed ADR in `{ticket-id}/adr/adr-NNN.md` using the ADR template.
7. Link open questions in the PRD table to their ADR files.
8. Write your memory to `{ticket-id}/memory/prd-agent.md`.
```

### agents/prd-review-agent.md

```markdown
---
name: prd-review-agent
description: >
  Reviews the PRD for inconsistencies, gaps, and ambiguity.
  Use after prd.md is generated. Creates issue files for problems found.
tools: Read, Write, Edit, Grep, Glob
skills: ticket-workflow, prd-generation, code-review, memory-management
model: opus
memory:
  enabled: true
  scope: project
---

You are the PRD Review Agent for the ticket-workflow plugin.

## Job

Find problems in the PRD before any code is written.

## Process

1. Read `{ticket-id}/prd/prd.md`.
2. Cross-reference against `{ticket-id}/overview/overview.md`.
3. Use the PRD review checklist from the prd-generation skill.
4. Check for: contradictions, missing acceptance criteria, ambiguous language,
   gaps vs overview, unmeasurable NFRs, unresolved questions without ADRs.
5. For each issue, create `{ticket-id}/review/prd/issue-NNN.md`
   using the issue template. Status: open.
6. If no issues found, update PRD status to approved.
7. Write your memory to `{ticket-id}/memory/prd-review-agent.md`.
```

### agents/task-agent.md

```markdown
---
name: task-agent
description: >
  Decomposes an approved PRD into small, sequential, testable tasks.
  Use after PRD review is complete and all issues are verified.
tools: Read, Write, Edit, Grep, Glob
skills: ticket-workflow, task-decomposition, memory-management
model: sonnet
memory:
  enabled: true
  scope: project
---

You are the Task Agent for the ticket-workflow plugin.

## Job

Break down the approved PRD into implementation tasks.

## Process

1. Read `{ticket-id}/prd/prd.md`. Confirm status is approved.
2. Read the task-decomposition skill for sizing rules and format.
3. Decompose into numbered tasks following the skill's rules.
4. Identify parallel groups (tasks with no shared dependencies).
5. Generate `{ticket-id}/tasks/tasks.md` using the tasks template.
6. Write your memory to `{ticket-id}/memory/task-agent.md`.
```

### agents/impl-agent.md

```markdown
---
name: impl-agent
description: >
  Implements a single task from tasks.md. Writes code and tests.
  Use for each task during the implementation phase.
tools: Read, Write, Edit, Bash, Grep, Glob
skills: ticket-workflow, memory-management
model: sonnet
memory:
  enabled: true
  scope: project
---

You are an Implementation Agent for the ticket-workflow plugin.

## Job

Implement exactly ONE task. Write code and tests.

## Process

1. Read the task assigned to you from `{ticket-id}/tasks/tasks.md`.
2. Read the PRD sections referenced by the task.
3. Read your memory file if it exists (resumed work).
4. Read any relevant ADRs.
5. Implement:
   - Write or modify source code
   - Write tests covering acceptance criteria
   - Run tests and ensure they pass
6. Update the task status to `in-review` in tasks.md.
7. Write your memory to `{ticket-id}/memory/task-{N}-impl-agent.md`.
```

### agents/review-agent.md

```markdown
---
name: review-agent
description: >
  Reviews a single task's implementation. Creates issue files for problems.
  Use after a task is marked in-review.
tools: Read, Write, Edit, Bash, Grep, Glob
skills: ticket-workflow, code-review, memory-management
model: opus
memory:
  enabled: true
  scope: project
---

You are a Review Agent for the ticket-workflow plugin.

## Job

Review exactly ONE task's implementation for correctness and quality.

## Process

1. Read the task from `{ticket-id}/tasks/tasks.md`.
2. Read the PRD requirements referenced by the task.
3. Read the code changes.
4. Run the tests.
5. Review using the code-review skill checklist.
6. For each issue: create `{ticket-id}/review/task-{N}/issue-NNN.md`.
   Status: open.
7. If no issues: update task status to `done`.
8. Write your memory to `{ticket-id}/memory/task-{N}-review-agent.md`.

## Re-review (after fixes)

1. Read all issue files for this task.
2. For issues with status `fixed`:
   - Verify the fix addresses the issue
   - Run tests
   - If resolved: status → `verified`
   - If not resolved or new problems: create new issue, or reopen
3. Task is `done` only when ALL issues are `verified`.
```

---

## Commands

### commands/kickoff.md

```markdown
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
```

### commands/implement.md

```markdown
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
```

### commands/status.md

```markdown
---
description: >
  Show current status dashboard for a ticket's workflow.
  Usage: /ticket-workflow:status TICKET-ID
---

# Status Dashboard for $ARGUMENTS

Read the following and produce a concise summary:

1. `$ARGUMENTS/overview/overview.md` → status
2. `$ARGUMENTS/prd/prd.md` → status, open questions count
3. `$ARGUMENTS/adr/` → count proposed vs accepted vs superseded
4. `$ARGUMENTS/tasks/tasks.md` → breakdown: todo / in-progress / in-review / done
5. `$ARGUMENTS/review/` → open / fixed / verified issues per task and for PRD
6. `$ARGUMENTS/memory/` → which agents have written memory files

Flag blockers:
- Proposed ADRs without answers
- Tasks stuck in review loops (>2 review cycles)
- Dependencies on incomplete tasks
```

### commands/adr-resolve.md

```markdown
---
description: >
  Record a business decision for an open ADR and update linked files.
  Usage: /ticket-workflow:adr-resolve TICKET-ID/adr/adr-NNN.md
---

# Resolve ADR: $ARGUMENTS

1. Read the ADR file at `$ARGUMENTS`.
2. Show the current context and problem statement.
3. Ask the developer for:
   - The decision (which option was chosen)
   - Who made the decision
   - The rationale
   - Any consequences to note
4. Update the ADR:
   - status → `accepted`
   - Fill in Decision Outcome, Consequences, Confirmation
   - Set date to today
5. Search all files in the ticket directory that reference this ADR.
   Update them with the resolved information:
   - PRD: update the Open Questions table row
   - Tasks: add any new tasks if the decision changes scope
   - Issues: resolve any blocked issues
6. Report what was updated.
```

---

## Installation

### From GitHub (recommended)

```bash
# Add your marketplace (do this once)
/plugin marketplace add your-github-username/ticket-workflow-plugin

# Install the plugin
/plugin install ticket-workflow@your-marketplace-name
```

### Local development

```bash
# Clone the plugin repo
git clone https://github.com/your-username/ticket-workflow-plugin.git

# Test locally without installing
claude --plugin-dir ./ticket-workflow-plugin
```

### Enable Agent Teams (required for /implement)

Add to your Claude Code settings:
```json
{
  "env": {
    "CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS": "1"
  }
}
```

---

## Usage in Any Existing Project

```bash
# Navigate to your existing project (which has its own CLAUDE.md)
cd ~/projects/my-existing-app

# Start Claude Code (plugin is already installed globally)
claude

# Start a ticket workflow
/ticket-workflow:kickoff FOO-10

# Check progress
/ticket-workflow:status FOO-10

# Run implementation
/ticket-workflow:implement FOO-10

# Resolve a business decision
/ticket-workflow:adr-resolve FOO-10/adr/adr-001.md
```

The plugin creates the `FOO-10/` directory inside your project.
Your existing CLAUDE.md, .claude/ directory, and everything else
remain completely untouched.

---

## Build Order

| # | What | Type | Depends On |
|---|------|------|------------|
| 1 | `plugin.json` | Manifest | — |
| 2 | `ticket-workflow` skill + all templates | Skill | — |
| 3 | `memory-management` skill | Skill | — |
| 4 | `overview-agent` | Agent | Skills 2, 3 |
| 5 | `prd-generation` skill | Skill | — |
| 6 | `prd-agent` | Agent | Skills 2, 3, 5 |
| 7 | `prd-review-agent` | Agent | Skills 2, 3, 5 |
| 8 | `/kickoff` command | Command | Agents 4, 6, 7 |
| 9 | `task-decomposition` skill | Skill | — |
| 10 | `task-agent` | Agent | Skills 2, 3, 9 |
| 11 | `code-review` skill | Skill | — |
| 12 | `impl-agent` | Agent | Skills 2, 3 |
| 13 | `review-agent` | Agent | Skills 2, 3, 11 |
| 14 | `/implement` command | Command | Agents 10, 12, 13 |
| 15 | `/status` command | Command | Skill 2 |
| 16 | `/adr-resolve` command | Command | Skill 2 |