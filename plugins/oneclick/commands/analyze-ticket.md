---
argument-hint: [ticket-id] [--auto-write]
description: Analyze LINEAR ticket requirements, explore codebase, and create implementation plan (Phase 1 only)
allowed-tools: Task, Grep, Glob, Read, TodoWrite, mcp__*
---

Perform comprehensive analysis and planning for LINEAR ticket: **$ARGUMENTS**

## Phase 1: Analysis & Planning

This command executes ONLY the analysis phase, allowing you to review the plan before implementation.

### Step 1: Parse Ticket Requirements

Launch `ticket-analyzer` agent:
- Fetch LINEAR ticket **$ARGUMENTS** using LINEAR MCP
- Read description, comments, related tickets, and dependencies
- Extract requirements and acceptance criteria
- Identify scope boundaries (related tickets are context only)
- Categorize ticket type
- Flag any ambiguities or blockers

### Step 2: Explore Codebase

Launch 2-3 `codebase-explorer` agents in parallel with different focuses:

**Agent 1 - Similar Features**:
- Find existing features similar to the ticket requirements
- Identify patterns used in comparable implementations
- Reference file:line locations

**Agent 2 - Architecture & Integration**:
- Understand overall architecture
- Map integration points and dependencies
- Identify affected layers/modules
- Reference file:line locations

**Agent 3 - Components & Conventions** (optional based on complexity):
- Identify reusable components
- Document code conventions and patterns
- Note testing approaches
- Reference file:line locations

**After agents complete**: Synthesize findings to identify key files and patterns for implementation.

### Step 3: Design Implementation

Launch 2-3 `code-architect` agents in parallel exploring different approaches:

**Agent 1 - Minimal Changes**:
- Pragmatic approach with minimal code changes
- Quick to implement, lower risk
- Focus on reusing existing patterns
- Specify files to modify with file:line references

**Agent 2 - Clean Architecture**:
- Maintainable, extensible approach
- May require more refactoring
- Focus on long-term code quality
- Specify files to modify with file:line references

**Agent 3 - Balanced Approach** (optional based on complexity):
- Mix of pragmatic and clean
- Balanced risk vs maintainability
- Specify files to modify with file:line references

**After agents complete**: Synthesize approaches and recommend best fit based on:
- Ticket complexity and story points
- Time constraints
- Long-term maintainability needs
- Team capacity

Create detailed TodoWrite with chosen approach.

### Step 4: Write Analysis to LINEAR

Write the analysis findings as a comment on the LINEAR ticket.

**LINEAR Writing Philosophy**:

The purpose of writing to LINEAR is to communicate a task clearly and provide context:
- Clear enough for the assignee to perform the work
- Enough context for teammates to understand what's being done
- Effective and quick to read

Balance clarity, context, and conciseness. Avoid over-sharing details that don't serve these goals.

**Comment Structure**:

```
**Complexity**: [Story points: 1, 2, 3, 5, 8, 13, etc.]

**Files to Modify**:
- `path/to/file1.js` - Brief description of changes
- `path/to/file2.js` - Brief description of changes

**Proposed Approach**:
[2-3 sentences describing the implementation strategy]

**Implementation Steps**:
1. Step one
2. Step two
3. Step three

**Dependencies/Blockers** (if any):
- List any blockers or dependencies

**Recommendation** (if applicable):
- Any recommendations for sub-tasks or approach
```

**Behavior**:
- **Default**: Show formatted analysis and wait for user approval before writing to LINEAR
- **With `--auto-write` flag**: Write directly to LINEAR without approval

**CRITICAL**: Without `--auto-write` flag, do not write to LINEAR until user approves.

## Deliverables

Final output includes:

1. **Requirements Analysis**
   - Ticket scope and acceptance criteria
   - Dependencies and blockers identified
   - Type of work (feature/bug/refactor)

2. **Codebase Analysis**
   - Affected files and components
   - Architecture patterns to follow
   - Existing similar features

3. **Implementation Plan**
   - Complexity sizing (story points)
   - Feasibility verdict
   - Proposed approach
   - Detailed todo list (via TodoWrite)
   - Risk assessment

4. **LINEAR Comment**
   - Analysis written to ticket (after approval or with --auto-write)
   - Formatted for clarity and team visibility

## Next Steps

After analysis complete:
- Use `/implement` to execute the plan (reads the LINEAR comment for context)
- Request plan adjustments if needed
- Make manual changes
- Cancel if not feasible

**Starting analysis for ticket $ARGUMENTS...**
