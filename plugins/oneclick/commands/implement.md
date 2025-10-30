---
argument-hint: [ticket-id] [--auto-commit]
description: Execute implementation plan by writing code and tests (Phase 2 only)
allowed-tools: Task, Read, Write, Edit, Bash, TodoWrite, TodoRead, mcp__*
---

Execute implementation for LINEAR ticket: **$ARGUMENTS**

## Phase 2: Implementation & Testing

This phase can run immediately after analysis or weeks later. The LINEAR ticket is the source of truth.

### Step 0: Gather Context from LINEAR

**Fetch ticket $ARGUMENTS using LINEAR MCP**:
- Read ticket description
- Read all comments (look for analysis from `/analyze-ticket`)
- Read related tickets and dependencies
- Understand scope and acceptance criteria

**Create/update TodoWrite**:
- If analysis comment exists: Use it to build todos
- If no analysis: Create todos from ticket description
- Include complexity, files to modify, implementation steps

**CRITICAL**: This step ensures implementation can work with or without prior `/analyze-ticket`.

---

### Step 1: Clarify Requirements

**CRITICAL: This is one of the most important steps.**

Review ticket and analysis for:
- Ambiguous requirements
- Missing acceptance criteria
- Technical uncertainties
- Dependency questions

**If clarification needed**:
- Ask user specific questions
- Wait for answers before proceeding
- Update TodoWrite based on answers

**If everything is clear**:
- Confirm understanding with user
- Show implementation plan from TodoWrite
- Get approval to proceed

**DO NOT START IMPLEMENTATION WITHOUT USER CONFIRMATION.**

---

### Step 2: Implement Features

Launch `code-implementer` agent:
- Work through TodoWrite systematically
- Write production-ready code following existing patterns
- Handle edge cases and errors
- Update TodoWrite as tasks complete
- Reference file:line locations in outputs

**Implementation Standards**:
- Match existing code style
- Proper error handling
- Security best practices
- Performance considerations
- Clear, maintainable code

### Step 3: Generate Tests

Launch `test-generator` agent:
- Create comprehensive test suites
- Follow project testing guidelines
- Test happy paths and edge cases
- Ensure all tests pass
- Reference file:line locations

**Testing Standards**:
- Independent, isolated tests
- Clear test names
- Proper setup/teardown
- Edge case coverage
- All tests passing

### Step 4: Quality Review

Launch 2-3 `code-reviewer` agents in parallel with different focuses:

**Agent 1 - Code Quality**:
- Focus on simplicity, DRY principles, elegance
- Check code duplication and maintainability
- Review naming and structure
- Report with confidence scoring (≥80)
- Include file:line references

**Agent 2 - Functional Correctness**:
- Focus on bugs, edge cases, logic errors
- Verify error handling and validation
- Check security vulnerabilities
- Report with confidence scoring (≥80)
- Include file:line references

**Agent 3 - Conventions & Patterns**:
- Focus on project conventions and patterns
- Check consistency with existing code
- Review abstractions and architecture fit
- Report with confidence scoring (≥80)
- Include file:line references

**All agents run automated checks**: linter, tests, build

**After agents complete**: Consolidate findings by severity (Critical, Important, Optional).

**Present findings and ask user to decide**:
- **Fix now** - Address issues immediately, then re-review
- **Fix later** - Document issues for future work
- **Proceed as-is** - Continue to PR with known issues

**CRITICAL**: Wait for user decision before proceeding.

---

### Step 5: Create Commits

**Commit Behavior**:

**Without `--auto-commit` flag**:
- Stop after quality review
- Tell user to commit changes manually
- Provide summary of files changed

**With `--auto-commit` flag**:
- Automatically create commit after user decision
- Use commit message format:
  ```
  [TICKET-ID]: Brief description

  - Implementation details
  - Files modified

  🤖 Generated with Claude Code
  Co-Authored-By: Claude <noreply@anthropic.com>
  ```
- Show commit hash after creation

**IMPORTANT**: This command does NOT create pull requests. Use `/create-pr` after commits are ready.

---

## Quality Gates

Implementation complete when:
- ✅ All TodoWrite items marked complete
- ✅ Code follows conventions
- ✅ Tests created and passing
- ✅ Quality review completed
- ✅ User decisions on issues documented

## Deliverables

Final output includes:
- Implementation summary with file:line references
- Files created/modified
- Test coverage report
- Quality review findings
- Todo completion status

## Next Steps

After implementation complete:
- Use `/review` to review local changes (diff with base branch)
- Use `/create-pr` to create pull request
- Use `/review-pr [pr-number]` to review the PR
- Or request additional changes

**Starting implementation for ticket $ARGUMENTS...**
