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
