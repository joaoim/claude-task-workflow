# ticket-workflow

A Claude Code plugin for Jira-driven, multi-agent development workflows. Each ticket gets its own scoped directory with structured phases: overview → PRD → tasks → implementation → review. Includes ADR tracking and per-agent memory.

Your existing project files are never touched.

---

## Installation

Copy the plugin contents into your global Claude Code config:

```bash
# Skills
cp -r skills/* ~/.claude/skills/

# Agents
cp -r agents/* ~/.claude/agents/

# Commands
mkdir -p ~/.claude/commands/ticket-workflow
cp commands/kickoff.md     ~/.claude/commands/ticket-workflow/kickoff.md
cp commands/implement.md   ~/.claude/commands/ticket-workflow/implement.md
cp commands/status.md      ~/.claude/commands/ticket-workflow/status.md
cp commands/adr-resolve.md ~/.claude/commands/ticket-workflow/adr-resolve.md
```

To use parallel agent execution during `/implement`, add this to your Claude Code settings:

```json
{
  "env": {
    "CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS": "1"
  }
}
```

---

## Usage

Open Claude Code in any project and run:

```
/ticket-workflow/kickoff FOO-123
```

That's it. The plugin takes over from there.

---

## Commands

### `/ticket-workflow/kickoff TICKET-ID`

Starts the full pre-implementation workflow for a ticket:

1. Scaffolds the `TICKET-ID/` directory
2. `@overview-agent` — captures business context and dev considerations
3. `@prd-agent` — generates a detailed PRD through interactive Q&A
4. `@prd-review-agent` — reviews the PRD, creates issues, loops until approved
5. `@task-agent` — decomposes the approved PRD into testable tasks

Stops after task decomposition for your review. Implementation is a separate step.

### `/ticket-workflow/implement TICKET-ID`

Runs the implementation phase:

- Spawns `@impl-agent` subagents in parallel (max 4), one per task
- As each task reaches `in-review`, `@review-agent` picks it up
- Fix/re-review loops run until all issues are verified
- Reports a summary when all tasks are done

### `/ticket-workflow/status TICKET-ID`

Prints a status dashboard showing:

- PRD status and open question count
- ADR breakdown (proposed / accepted / superseded)
- Task breakdown (todo / in-progress / in-review / done)
- Open, fixed, and verified issues per task
- Which agents have written memory files
- Active blockers

### `/ticket-workflow/adr-resolve TICKET-ID/adr/adr-NNN.md`

Records a business decision for an open ADR:

1. Shows the current context and options
2. Asks for the decision, decision-maker, rationale, and consequences
3. Updates the ADR to `accepted`
4. Propagates the decision to linked PRD questions, tasks, and issues

---

## Agents

| Agent | Model | Job |
|---|---|---|
| `@overview-agent` | Sonnet | Creates `overview.md` from Jira ticket + dev input |
| `@prd-agent` | Opus | Generates `prd.md` via interactive Q&A; creates ADRs for unresolved questions |
| `@prd-review-agent` | Opus | Reviews PRD for gaps, ambiguity, and contradictions |
| `@task-agent` | Sonnet | Decomposes approved PRD into numbered, testable tasks |
| `@impl-agent` | Sonnet | Implements one task; writes code and tests |
| `@review-agent` | Opus | Reviews one task's implementation; creates issue files |

---

## Directory Structure Per Ticket

```
TICKET-ID/
├── overview/
│   └── overview.md          # business context, constraints, risks, open questions
├── prd/
│   └── prd.md               # full spec with acceptance criteria and NFRs
├── adr/
│   └── adr-001.md           # one file per unresolved decision
├── tasks/
│   └── tasks.md             # numbered tasks with dependencies and acceptance criteria
├── review/
│   ├── prd/
│   │   └── issue-001.md     # PRD review issues
│   └── task-1/
│       └── issue-001.md     # implementation review issues
└── memory/
    ├── overview-agent.md    # per-agent persistent context
    ├── prd-agent.md
    ├── prd-review-agent.md
    ├── task-agent.md
    ├── task-1-impl-agent.md
    └── task-1-review-agent.md
```

---

## Skills

| Skill | Used By | Purpose |
|---|---|---|
| `ticket-workflow` | All agents | Phase flow, status rules, directory layout, templates |
| `memory-management` | All agents | How to read/write memory files |
| `prd-generation` | `@prd-agent`, `@prd-review-agent` | Question framework, quality bar, review checklist |
| `task-decomposition` | `@task-agent` | Sizing rules, dependency graphs, parallel execution waves |
| `code-review` | `@review-agent`, `@prd-review-agent` | Review checklist, severity definitions, issue format |

---

## Status Flows

```
PRD:   draft → in-review → approved
Task:  todo → in-progress → in-review → done
Issue: open → fixed → verified
ADR:   proposed → accepted → superseded | deprecated
```

---

## Rules

- The orchestrator never writes code or documents — it only coordinates agents.
- No agent guesses at business decisions. Unresolved questions become proposed ADRs.
- Each agent reads its memory file at the start and writes it at the end of every session.
- Tasks must be small enough for one agent session and independently testable.
- A task is only `done` when all its issues are `verified`.
- Maximum 4 parallel agents to control token cost.
