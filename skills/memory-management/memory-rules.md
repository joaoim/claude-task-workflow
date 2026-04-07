# Memory Rules

## Mandatory Behavior

1. **Read-first:** Every agent reads its memory file before starting work.
2. **Write-last:** Every agent writes its memory file after completing work.
3. **No skipping:** Memory write is not optional, even if the session was short.

## Content Rules

1. **Key Context is sacred:** The Key Context section must always contain the
   3-5 most critical facts. If resuming, this is all you need to read.
2. **Decisions need rationale:** Never record a decision without explaining why.
3. **Problems need resolution:** Record the resolution or mark as open.
4. **Assumptions are risks:** Every assumption is something that could invalidate work.
5. **Handoff notes are for others:** Write them as if briefing someone who has
   never seen this ticket before.

## Size Management

- Target: under 200 lines per memory file.
- If approaching the limit, summarize the oldest Work Log entries.
- Archive detailed logs to `memory/{agent-name}-archive-{N}.md` if needed.
- The Key Context section should never be summarized — it stays current.

## File Format

Use the memory template from the ticket-workflow skill's templates directory.
All memory files must follow the same structure for consistency.

## Cross-Agent Memory

- Agents should read other agents' memory files when relevant context is needed.
- Never modify another agent's memory file.
- Reference other agents' findings in your own memory when relevant.
