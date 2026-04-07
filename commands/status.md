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
