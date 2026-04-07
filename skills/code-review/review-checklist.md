# Review Checklist

## Pre-Review

- [ ] Read the task description and acceptance criteria
- [ ] Read the relevant PRD sections
- [ ] Read any related ADRs
- [ ] Read the impl-agent's memory file for context on decisions made

## Correctness

- [ ] Each acceptance criterion is satisfied by the implementation
- [ ] Logic matches the PRD specification (not just "works")
- [ ] Return values and outputs match expected formats
- [ ] State transitions follow defined flows

## Edge Cases

- [ ] All PRD-defined edge cases are handled
- [ ] Boundary conditions are tested
- [ ] Empty/null/missing input is handled
- [ ] Concurrent access scenarios are considered (if applicable)

## Error Handling

- [ ] Errors are caught at appropriate levels
- [ ] Error messages are meaningful (not swallowed silently)
- [ ] Failure modes from PRD are implemented
- [ ] Partial failure scenarios are handled

## Security

- [ ] User input is validated and sanitized
- [ ] Authentication/authorization checks are in place
- [ ] No sensitive data in logs or error messages
- [ ] No hardcoded credentials or secrets

## Performance

- [ ] No N+1 query patterns
- [ ] No unnecessary loops over large collections
- [ ] No unbounded memory growth
- [ ] Database queries use appropriate indexes

## Tests

- [ ] Tests exist for each acceptance criterion
- [ ] Tests cover happy path
- [ ] Tests cover error/edge cases
- [ ] Tests are deterministic (no flaky tests)
- [ ] Existing test suite still passes (no regressions)

## Code Quality

- [ ] Code is readable without excessive comments
- [ ] Naming is clear and consistent
- [ ] No unnecessary duplication
- [ ] Complexity is appropriate for the problem
