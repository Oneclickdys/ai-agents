---
argument-hint: [ticket-id]
description: Execute complete workflow - analyze, implement, create PR, and review PR
allowed-tools: Task, Grep, Glob, Read, Edit, Write, Bash, TodoWrite, TodoRead, mcp__*
---

Execute complete workflow for LINEAR ticket: **$ARGUMENTS**

## Workflow Overview

This command runs the 4 main workflow commands sequentially:

1. **`/analyze-ticket`** - Analysis & planning
2. **`/implement`** - Implementation & testing
3. **`/create-pr`** - Pull request creation
4. **`/review-pr`** - PR review & feedback

All checkpoints and user interactions are delegated to the individual commands.

---

## Sequential Execution

### Step 1: Run Analysis

Launch `/analyze-ticket $ARGUMENTS --auto-write`:
- Analyzes LINEAR ticket
- Explores codebase
- Designs implementation
- Writes analysis to LINEAR automatically

**Checkpoint**: Command handles user approval for plan.

---

### Step 2: Run Implementation

Launch `/implement $ARGUMENTS --auto-commit`:
- Reads ticket and analysis from LINEAR
- Clarifies requirements if needed
- Implements features
- Generates tests
- Reviews code quality
- Auto-commits after approval

**Checkpoints**: Command handles clarification questions and fix decisions.

---

### Step 3: Create Pull Request

Launch `/create-pr`:
- Creates PR from current branch
- Handles submodule PRs if needed
- Generates comprehensive PR description
- Links to LINEAR ticket

**Checkpoint**: Command verifies commits exist before PR creation.

---

### Step 4: Review Pull Request

After PR is created, launch `/review-pr [pr-number]`:
- Reviews the PR changes
- Identifies issues
- Provides feedback
- Recommends approval/changes

**Note**: PR number is obtained from Step 3 output.

---

## Automation Level

This workflow uses automation flags for streamlined execution:
- `--auto-write`: Analysis writes to LINEAR without approval
- `--auto-commit`: Implementation commits without manual intervention

**Manual checkpoints remain**:
- Plan review (in analyze-ticket)
- Clarification questions (in implement)
- Fix decisions (in implement)

---

## If Any Step Fails

The workflow stops at the failing command:
- Review error output from that command
- Fix issues manually
- Continue from that step or restart workflow

---

## Next Steps

After workflow completes:
- PR is created and reviewed
- Address any review feedback
- Track PR through team review process
- Merge when approved

**Starting workflow for ticket $ARGUMENTS...**
