---
argument-hint: --title [title] --description [description] --priority [priority] --duedate [due-date]
description: Create a Linear (TANAI) task with enhanced description based on repository analysis
allowed-tools: Grep, Glob, Read, Task, mcp__linear__*
response-language: es
---

I need to create a Linear task with the following arguments: $ARGUMENTS

Please follow this workflow:

## Phase 1: Analysis and Enhancement

1. Parse arguments: extract `--title` and `--description`
2. Improve the technical description using the provided title and description
3. Analyze the repository to identify:
    - Relevant files and functions
    - Related documentation
    - Existing similar implementations/patterns
    - Database schema or configurations (Prisma)
4. Scan documentation in `doc/` and `docs/` for technical requirements

## Phase 2: Task Description Generation

Using `.claude/task-template.md`, create a description including:

- Summary (one line)
- End goal/objective
- Technical context and implementation approach
- Relevant code files (with brief description)
- Relevant documentation paths (and why)
- Files to create/modify
- Implementation notes
- Acceptance criteria (measurable)

## Phase 3: Create in Linear

1. Present the enhanced description
2. Automatically create the Linear task using MCP:
    - Tool: `mcp__linear__createIssue`
    - Project: `TANAI`
    - Use `--priority` and `--duedate` when provided
    - Return the created task ID/URL

IMPORTANT: After presenting the enhanced description, proceed to create the Linear task without waiting for approval (use defaults).

Start by parsing `--title` and `--description`, then begin repository analysis.
