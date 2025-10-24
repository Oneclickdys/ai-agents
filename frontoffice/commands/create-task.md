---
argument-hint: --title [task title] --description [task description] --priority [task priority] --duedate [task due-date]
description: Create a JIRA task with enhanced description based on codebase analysis
allowed-tools: Grep, Glob, Read, Task, mcp__atlassian__*
---

I need to create a JIRA task with the following details from the arguments: $ARGUMENTS

Please help me by following this workflow:

## Phase 1: Analysis and Enhancement

1. **Parse Arguments**: Extract the --title and --description from the provided arguments
2. **Improve Prompt**: Based on the title and description, create a more detailed and technical prompt
3. **Analyze Codebase**: Search through the repository to find:
   - Relevant code files and functions
   - Related documentation
   - Similar patterns or existing implementations
   - Database schemas or configurations
4. **Scan Documentation**: Review relevant docs to determine technical requirements

## Phase 2: Task Description Generation

**CRITICAL** the template at MUST be followed `.claude/task-template.md`, create a comprehensive task description that includes:
- **Summary**: Brief one-line summary
- **End Goal/Objective**: Clear desired outcome
- **Technical Context**: Architecture and implementation approach
- **Relevant Code Files**: List with descriptions
- **Relevant Documentation Paths**: List with relevance notes
- **Potentially New Files or To Be Modified**: What needs to be created/changed
- **Implementation Notes**: Technical considerations from analysis
- **Acceptance Criteria**: Specific measurable outcomes

## Phase 3: Review and JIRA Creation

1. **Present Enhanced Description**: Show the complete task description for review
2. **Create JIRA Issue Automatically**: Immediately create the JIRA task using:
   - Using the JIRA MCP tool. Tool name: createJiraIssue Full name: mcp__atlassian__createJiraIssue
   - Project from `.claude/jira-config.yaml`
   - Enhanced description as the task body
   - Assign priority and duedate if provided in the --priority and --duedate flags
   - Return the created task ID/URL to the user

**IMPORTANT**: After presenting the enhanced description, immediately proceed to create the JIRA task automatically. Do not wait for approval - use the config defaults.

Start by parsing the title and description from the arguments, then begin the codebase analysis.