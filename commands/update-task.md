---
argument-hint: [TANAI-XXXX] --additional-context [optional context]
description: Update a Linear (TANAI) task with repository-driven technical analysis
allowed-tools: Grep, Glob, Read, Task, mcp__linear__*
response-language: es
---

I need to update an existing Linear task: $ARGUMENTS

Please follow this workflow:

## Phase 1: Retrieve and Analyze

1. Parse arguments: extract ID (`TANAI-XXXX`) and `--additional-context` if present
2. Fetch current task details via Linear MCP
3. Extract:
    - Title/summary
    - Description
    - Current status and priority
    - Existing technical context

## Phase 2: Repository Analysis and Enhancement

1. Deepen understanding using the provided context
2. Analyze the repository for:
    - Relevant files/functions
    - Related documentation
    - Similar implementations or dependencies
    - Prisma schema and recent migrations
3. Review docs under `doc/` and `docs/`
4. Incorporate `--additional-context`

## Phase 3: Enhanced Description Generation

Using `.claude/task-template.md`, produce an updated description with:

- Summary (refined if needed)
- Clear end goal
- Technical context (architecture and approach)
- Relevant files (with brief explanation)
- Relevant documentation (with rationale)
- Files to create/modify
- Implementation notes
- Updated acceptance criteria (preserving useful existing ones)
- Additional findings

## Phase 4: Update in Linear

1. Present the enhanced description
2. Update the Linear task via MCP:
    - Tool: `mcp__linear__updateIssue`
    - Preserve priority and due date unless specified otherwise
    - Add note: "✨ Enhanced with technical analysis and codebase review"
    - Confirm successful update

IMPORTANT:

- Preserve existing metadata (assignee, priority, due date) unless instructed
- Prefer appending/expanding over replacing if content is valuable

Start by parsing the Linear ID and any additional context, then fetch current task details.
