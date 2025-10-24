---
argument-hint: [linear-task-id] --additional-context [optional context for AI to consider]
description: Update existing LINEAR task with enhanced analysis based on codebase review
allowed-tools: Grep, Glob, Read, Task, mcp__atlassian__*
---

I need to update an existing LINEAR task with enhanced technical details. The task ID provided is: $ARGUMENTS

Please help me by following this workflow:

## Phase 1: Retrieve and Analyze

1. **Parse Arguments**: Extract the LINEAR task ID and any additional context from the arguments
2. **Fetch Existing Task**: Retrieve the current LINEAR task details using:
   - Tool: Linear MCP
   - Cloud ID from MCP tools
3. **Extract Current Details**: Parse the existing:
   - Title/Summary
   - Description
   - Current status and priority
   - Any existing technical context

## Phase 2: Codebase Analysis and Enhancement

1. **Improve Understanding**: Based on the existing title, description, and any additional context provided, create a more detailed technical analysis
2. **Analyze Codebase**: Search through the repository to find:
   - Relevant code files and functions
   - Related documentation
   - Similar patterns or existing implementations
   - Database schemas or configurations
   - Recent changes that might affect implementation
3. **Scan Documentation**: Review relevant docs to determine technical requirements
4. **Consider Additional Context**: Incorporate any additional insights provided in the --additional-context flag

## Phase 3: Enhanced Description Generation

Using the template from `.claude/task-template.md`, create an updated comprehensive task description that includes:
- **Summary**: Keep existing or refine if needed
- **End Goal/Objective**: Clear desired outcome (enhanced from original)
- **Technical Context**: Architecture and implementation approach based on codebase analysis
- **Relevant Code Files**: List with descriptions and line numbers where applicable
- **Relevant Documentation Paths**: List with relevance notes
- **Potentially New Files or To Be Modified**: What needs to be created/changed
- **Implementation Notes**: Technical considerations from analysis
- **Acceptance Criteria**: Specific measurable outcomes (preserve existing, add new if needed)
- **Additional Insights**: Any new findings from the additional context provided

## Phase 4: Update LINEAR Task

1. **Present Enhanced Description**: Show the complete updated task description for review
2. **Update LINEAR Issue Automatically**: Immediately update the LINEAR task using:
   - Tool: Linear MCP
   - Preserve existing priority and due date unless specifically changed
   - Update description with enhanced technical analysis
   - Add a note indicating: "✨ Enhanced with technical analysis and codebase review"
   - Return confirmation of successful update

**IMPORTANT**:
- Preserve all existing metadata (assignee, priority, due date, etc.) unless explicitly instructed to change
- Append to existing description rather than replacing if there's valuable existing content
- After presenting the enhanced description, immediately proceed to update the JIRA task automatically

Start by parsing the LINEAR task ID and any additional context, then fetch the existing task details.