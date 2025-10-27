---
argument-hint: [linear-task-id]
description: Execute agentic workflow for LINEAR task implementation with 3-stage process
allowed-tools: Task, Grep, Glob, Read, Edit, MultiEdit, Write, Bash, TodoWrite, TodoRead, mcp__context7__resolve-library-id, mcp__context7__get-library-docs,glab, git, mcp__atlassian__*, mcp__ide__*
---

I will execute a comprehensive 3-stage workflow to implement the LINEAR task: **$ARGUMENTS**

**CRITICAL**: Perform environment setup before executing the 3 stage plan

## Repository Structure Awareness

### Frontend Architecture

This repository uses a **Git submodule** architecture:

- **Parent repo**: `tangerine-frontoffice` - Main application (main branch: `main`, PR target: `QA`)
- **Submodule**: `src/_core` - shared components (main branch: `main-core`, PR target: `QA-core`)

### Ecosystem Context

This is the **frontend** repository. Related repositories:

- **Backend APIs**:
  - `i2c_v2` - Core API at `/Users/juan/OC/Tangerine/i2c_v2`
  - `tangerine-api-v3` - Extended API at `/Users/juan/OC/Tangerine/tangerine-api-v3`

### Task Scope Identification

When implementing LINEAR tasks:

1. **Frontend-only tasks** (most common):

   - Identify if changes affect submodule (`src/_core`) or parent files
   - For submodule changes: create branches in submodule
   - For parent-only changes: create branch only in parent

2. **Full-stack tasks** (require backend changes):
   - Check LINEAR task description for backend requirements
   - If API changes needed:
     - ⚠️ **STOP**: Coordinate with backend team first
     - Backend PR should be merged and deployed before frontend work
     - Document required API endpoints/changes
   - Then proceed with frontend implementation

**Note**: Most frontend tasks do NOT require backend changes. Only proceed to backend if explicitly required.

## Environment Setup

First, I'll set up the development environment:

1. Retrieve LINEAR details using MCP tools
2. Fetch LINEAR task details from **$ARGUMENTS** and requirements using LINEAR MCP tools
3. Fetch latest changes in parent: `git fetch origin`
4. Checkout to main and pull latest in parent: `git checkout main && git pull`
5. Check and update submodule if needed:
   ```bash
   git submodule update --init --recursive
   cd src/_core
   git checkout main-core
   git pull origin main-core
   cd ../..
   ```
6. **After analyzing the task**, determine if changes will be in:

   - **Submodule only**: Create branch in `src/_core` from `main-core`, then create matching branch in parent
   - **Parent only**: Create branch in parent from `main`. If in parent only is required crate a branch to update the submodule reference, dont create branch for this.
   - **Both**: Create branches in both repos

7. Create feature branch(es):
   - If submodule affected: `cd src/_core && git checkout -b tan-XXXX-description && cd ../..`
   - If parent affected: `git checkout -b tan-XXXX-description`

Pass the LINEAR task details to stage 1.
This workflow follows these stages:

## Stage 1: Analysis & Planning Agent (Task Analysis & Todo Configuration)

Using the `analysis-planning-agent`, I will:

- Parse acceptance criteria and technical specifications
- **Identify task scope**:
  - Frontend-only vs Full-stack
  - Parent repo vs Submodule (`src/_core`)
  - Backend API dependencies (if any)
- Identify task type (feature, bug fix, refactor, etc.)
- Conduct codebase analysis to identify affected files and components
- **Check for backend requirements**:
  - If API changes needed: document required endpoints
  - If backend-dependent: note that backend must be ready first
  - If frontend-only: proceed with full implementation
- Assess technical feasibility and provide implementation strategy
- Design implementation approach based on existing architecture patterns
- Create detailed todo list with specific, actionable tasks using TodoWrite
- Sequence tasks logically following the codebase's dependency flow
- Provide implementation guidance for each major task

**If backend changes are required**: The plan should include:

- List of required API endpoints/changes
- Note that backend work is out of scope for this agent
- Frontend implementation assumes backend is ready

### 🛑 CHECKPOINT: Plan Review & Approval

**STOP HERE** and present the analysis and plan to the user:

1. **Present the Analysis Summary**:

   - Task scope (frontend-only vs full-stack, parent vs submodule)
   - Affected files and components
   - Technical approach and architecture decisions
   - Backend dependencies (if any)

2. **Present the Todo List**:

   - Show the complete todo list created by the planning agent
   - Explain the sequence and rationale for each major task
   - Highlight any risks, assumptions, or decision points

3. **Wait for User Approval**:

   - Ask: "Does this plan look good? Would you like me to proceed with implementation, or should I adjust anything?"
   - User can:
     - ✅ **Approve**: Proceed to Stage 2 (Implementation)
     - 🔄 **Request changes**: Iterate on the plan with specific feedback
     - ❌ **Cancel**: Stop the workflow

4. **Handle Feedback**:
   - If user requests changes, update the plan and todo list accordingly
   - Re-present the revised plan for approval
   - Only proceed to Stage 2 after explicit user approval

## Stage 2: Implementation & Testing Agent (Code Writing & Verification)

**⚠️ Only proceeds after user approval from Stage 1 checkpoint**

Using the `implementation-agent`, I will:

- Execute planned tasks systematically following the todo list
- Write code following existing patterns and conventions discovered in analysis
- Implement features according to specifications and acceptance criteria
- Handle edge cases and error scenarios with proper validation
- **Create comprehensive test suites** by:
  - Following testing guidelines and conventions detailed in `.cursor/rules/unit-testing.mdc`
  - Generating unit tests, integration tests, and mock interfaces as specified
  - Ensuring all implementation meets project testing standards and coverage requirements
  - Running test suites and handling any failures according to project testing protocols
  - Validating code quality and correctness through systematic testing approach
- Track progress continuously using TodoWrite for both implementation and testing tasks
- Ensure no linter issues and all tests pass before completing
- **Use `/commit` command to create proper git commits with LINEAR references**
  - **IMPORTANT**: If changes are in `src/_core/`, commit in submodule FIRST. If the only change in parent is the submodule reference, DONT make the commit. The submodule reference in the parent will be managed by the reviewer of the PR manually
  - The `/commit` command is aware of the submodule structure and will handle this correctly

## Stage 3: Reviewer Agent (Pull Request Creation)

I will receive input from:

- **Analysis & Planning Agent**: Original requirements, scope, and implementation approach
- **Implementation Agent**: Actual code changes, features added, and test results

Using the `review-agent`, I will:

- Gather implementation summary from all previous agents' outputs
- Compile test results and verification status from the Implementation Agent
- **Use `/create-mr` command to**:
  - Run final quality checks (lint, tests)
  - Push the feature branch(es) to GitHub remote
  - **If changes are in submodule**: Create TWO pull requests:
    1. Submodule PR (base: `main-core`) with technical implementation details
    2. Parent PR (base: `main`) referencing the submodule PR
  - **If changes are parent-only**: Create ONE pull request (base: `main`)
  - Create comprehensive pull request(s) with LINEAR references
  - Include test results and implementation summary
- Set appropriate reviewers based on project conventions
- Return the pull request URL(s) for user review and approval

**Note**: The `/create-mr` command understands the submodule structure and will guide PR creation correctly

## Iterative Process

After implementation, you can request modifications:

- Changes will be implemented through the same agent workflow
- New commits will be added iteratively
- Each iteration maintains code quality and follows established patterns

## Summary of Workflow with Approval Gate

```
┌─────────────────────────────────────────────────────────────┐
│ Stage 1: Analysis & Planning Agent                          │
│ - Analyze LINEAR task                                       │
│ - Identify scope and affected components                    │
│ - Create implementation plan and todo list                  │
└─────────────────────────────────────────────────────────────┘
                            │
                            ▼
┌─────────────────────────────────────────────────────────────┐
│ 🛑 CHECKPOINT: Plan Review & Approval                       │
│ - Present analysis summary                                  │
│ - Show todo list and approach                               │
│ - Wait for user approval                                    │
│                                                              │
│ User Options:                                               │
│   ✅ Approve → Continue to Stage 2                          │
│   🔄 Iterate → Adjust plan and re-present                   │
│   ❌ Cancel → Stop workflow                                 │
└─────────────────────────────────────────────────────────────┘
                            │
                            ▼ (Only after approval)
┌─────────────────────────────────────────────────────────────┐
│ Stage 2: Implementation & Testing Agent                     │
│ - Execute todos systematically                              │
│ - Write code and tests                                      │
│ - Run linter and tests                                      │
│ - Create commits with /commit command                       │
└─────────────────────────────────────────────────────────────┘
                            │
                            ▼
┌─────────────────────────────────────────────────────────────┐
│ Stage 3: Reviewer Agent                                     │
│ - Gather implementation summary                             │
│ - Run final quality checks                                  │
│ - Create PR(s) with /create-mr command                      │
│ - Return PR URL(s) for review                               │
└─────────────────────────────────────────────────────────────┘
```

**Starting workflow for task: $ARGUMENTS**

Let me begin by launching the Task Analysis Agent to fetch and analyze the LINEAR task details...
