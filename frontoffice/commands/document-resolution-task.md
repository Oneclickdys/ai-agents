---
argument-hint: [linear-task-id] --additional-context [optional context for AI to consider]
description: Update existing LINEAR task with a comment explaining the solution to fix the task
allowed-tools: Grep, Glob, Read, Task, mcp__context7__resolve-library-id, mcp__context7__get-library-docs, mcp__linear__*, mcp__github__*, Bash
---

# Document Resolution Task Command

## Purpose

This command analyzes a completed Linear task by examining its associated pull requests or commits, then adds a comprehensive comment to the task explaining how the solution was implemented.

## Workflow

1. **Retrieve Task Information**: Fetch the Linear task details using the provided task ID
2. **Identify Related Changes**: Find associated PRs or commits linked to the task
3. **Analyze Implementation**: Review the code changes, files modified, and solution approach
4. **Consult Documentation** (if needed): Use Context7 to fetch relevant library documentation to better understand the implementation
5. **Document Solution**: Add a structured comment to the Linear task explaining the resolution

## Comment Template

All resolution comments must follow this structure:

```markdown
## Resolution Documentation

### Reference
- **Commit/PR ID**: [commit-hash or PR-number]
- **Link**: [URL to commit or PR]

### Modified Files
- `path/to/file1.js`
- `path/to/file2.scss`
- `path/to/file3.test.js`

### Problem Identified
[Clear description of the issue that was addressed, including symptoms, error messages, or unexpected behaviors that users experienced]

### Solution Applied
[Detailed explanation of the technical solution implemented, including:
- Approach taken to solve the problem
- Key code changes made
- Libraries or patterns used
- Design decisions and rationale]

### Behavior After Fix
[Description of how the application behaves after the fix is applied:
- What now works correctly
- Changes in user experience
- Performance improvements (if applicable)]

### Impact
[Assessment of the fix's scope and implications:
- **Scope**: Which parts of the application are affected
- **Risk Level**: Low/Medium/High
- **Testing**: What was tested and how
- **Side Effects**: Any other changes or considerations
- **Breaking Changes**: Yes/No - with explanation if yes]

---
*Documentation generated automatically via Claude Code*
```

## Usage Instructions

When this command is invoked:

1. Parse the Linear task ID from the command arguments
2. Use `mcp__linear__get_issue` to fetch task details
3. Extract linked PR or commit information from the task
4. Use git commands (`git show`, `git log`) to examine commit details
5. Use GitHub tools if PR information is available
6. Read modified files to understand the changes
7. If complex libraries are involved, use Context7 to fetch documentation
8. Generate a comment following the template structure
9. Use `mcp__linear__create_comment` to add the documentation to the task

## Example Execution

```bash
/document-resolution-task TAN-1234 --additional-context "Focus on the Redux state management changes"
```

This would:
- Fetch Linear task TAN-1234
- Find the associated PR or commits
- Analyze the Redux-related changes
- Generate and post a comprehensive resolution comment

## Best Practices

- Be thorough but concise in explanations
- Use technical language appropriate for the development team
- Include relevant code snippets when helpful (in backticks)
- Link to external resources or documentation when applicable
- Highlight any follow-up tasks or known limitations
- Always verify commit/PR IDs are correct before posting
