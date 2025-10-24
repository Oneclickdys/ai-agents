# Oneclick Plugin

Comprehensive LINEAR ticket implementation workflow for [Claude Code](https://github.com/anthropics/claude-code) with specialized agents for analysis, implementation, testing, and review.

> Built following [Anthropic's plugin best practices](https://github.com/anthropics/claude-code/tree/main/plugins).

## Overview

This plugin provides a complete workflow for implementing LINEAR tickets through four main commands:

1. **`/analyze-ticket`**: Parse requirements, explore codebase, design implementation
2. **`/implement`**: Write code, create tests
3. **`/create-pr`**: Create pull request
4. **`/review-pr`**: Review and validate PR quality

## Installation

```bash
# Add marketplace
/plugin marketplace add Oneclickdys/ai-agents

# Install plugin
/plugin install oneclick@oneclick
```

The LINEAR MCP server will be automatically configured during installation. If you need to configure it manually:

```json
{
  "mcpServers": {
    "linear": {
      "command": "npx",
      "args": ["-y", "mcp-remote", "https://mcp.linear.app/sse"]
    }
  }
}
```

Add this to your Claude Code settings at `~/.claude/settings.json`.

---

## Commands

### Workflow Commands

#### `/workflow [TICKET-ID]`
**Complete 4-step workflow** from analysis to PR review.

**Usage**:
```bash
/workflow TAN-123
```

**What it does**:
1. Runs `/analyze-ticket TAN-123`
2. Runs `/implement TAN-123`
3. Runs `/create-pr`
4. Runs `/review-pr [pr-number]`

Each command runs sequentially to provide a complete ticket implementation workflow.

---

#### `/analyze-ticket [TICKET-ID] [--auto-write]`
**Analysis & Planning**: Analyze ticket and create implementation plan.

**Usage**:
```bash
/analyze-ticket TAN-123
/analyze-ticket TAN-123 --auto-write  # Writes analysis to LINEAR as comment
```

**Outputs**:
- Requirements analysis
- Codebase exploration
- Implementation plan with todos
- Feasibility assessment
- Optional: LINEAR comment with analysis findings (with `--auto-write`)

**Next steps**: Run `/implement TAN-123` to execute the plan

---

#### `/implement [TICKET-ID] [--auto-commit]`
**Implementation & Testing**: Execute implementation plan by reading LINEAR ticket and comments.

**Usage**:
```bash
/implement TAN-123
/implement TAN-123 --auto-commit  # Auto-commits without asking
```

**What it does**:
- Reads LINEAR ticket and all comments (including analysis if available)
- Clarifies requirements with user before starting
- Writes production code following patterns
- Creates comprehensive test suites
- Reviews code quality with parallel agents
- Creates commits (asks for approval unless `--auto-commit`)

**Works standalone**: Can run immediately after `/analyze-ticket` or weeks later (reads from LINEAR)

**Next steps**: Run `/create-pr` to create pull request

---

#### `/review`
**Local Changes Review**: Review uncommitted or committed local changes by comparing with base branch.

**Usage**:
```bash
/review
```

**What it does**:
- Compares local changes with base branch
- Runs linter and tests
- Performs code quality review
- Reports issues and suggestions

**Note**: This is NOT for reviewing PRs - use `/review-pr` for that

**Next steps**: Run `/create-pr` after addressing feedback

---

### Utility Commands

#### `/create-pr`
Create pull request with comprehensive description and LINEAR references.

**Usage**:
```bash
/create-pr
```

---

#### `/review-pr [pr-number]`
Review an existing pull request and provide detailed feedback.

**Usage**:
```bash
/review-pr 123
```

**What it does**:
- Fetches PR details using `gh` CLI
- Reviews code changes
- Checks for bugs, quality issues, and convention adherence
- Provides actionable feedback

---

#### `/create-task`
Create new LINEAR task with enhanced description based on codebase analysis.

---

#### `/update-task [TICKET-ID]`
Update existing LINEAR task with enhanced analysis based on codebase review.

---

## Agents

### Analysis Phase Agents

#### `ticket-analyzer`
**Role**: Parse LINEAR requirements

**What it does**:
- Fetches ticket details from LINEAR
- Extracts acceptance criteria
- Identifies dependencies
- Flags ambiguities

**Tools**: Read, LINEAR MCP

---

#### `codebase-explorer`
**Role**: Understand project structure

**What it does**:
- Locates relevant existing code
- Maps architecture and patterns
- Identifies integration points
- Documents conventions

**Tools**: Glob, Grep, Read

---

#### `code-architect`
**Role**: Design implementation approach

**What it does**:
- Assesses technical feasibility
- Designs step-by-step plan
- Creates detailed todo list
- Specifies files to modify

**Tools**: Read, TodoWrite

---

### Implementation Phase Agents

#### `code-implementer`
**Role**: Write production code

**What it does**:
- Executes todos in order
- Writes code following patterns
- Handles edge cases
- Tracks progress

**Tools**: Read, Write, Edit, Bash, Glob, Grep

---

#### `test-generator`
**Role**: Create comprehensive tests

**What it does**:
- Generates test suites
- Tests happy paths and edge cases
- Ensures all tests pass
- Meets coverage requirements

**Tools**: Read, Write, Edit, Bash

---

### Review Phase Agents

#### `code-reviewer`
**Role**: Validate code quality

**What it does**:
- Runs linter and tests
- Checks conventions
- Identifies bugs
- Reports quality issues

**Tools**: Read, Grep, Bash

---

#### `pr-creator`
**Role**: Create pull requests

**What it does**:
- Gathers implementation context
- Creates comprehensive PR description
- Links to LINEAR ticket
- Submits PR

**Tools**: Read, Bash, gh CLI

---

## Skills

Skills provide platform-specific knowledge that agents autonomously use based on context.

### Frontoffice Skills

#### `crud-operations`
CRUD patterns for API operations

**When used**:
- Implementing data fetching
- Creating API calls
- Handling errors

**Key patterns**:
- Standard CRUD module structure
- Error handling
- Retry logic
- Optimistic updates

---

#### `react-component-patterns`
React component architecture

**When used**:
- Creating React components
- Understanding component structure
- Implementing UI features

**Key patterns**:
- Component types (atoms, components, compositions)
- PropTypes and validation
- Component best practices

---

#### `global-state-management`
Redux/Context state patterns

**When used**:
- Managing global state
- Creating Redux actions/reducers
- Sharing state across components

**Key patterns**:
- Redux structure
- Action creators
- Reducers and selectors

---

#### `frontend-testing`
Jest & React Testing Library patterns

**When used**:
- Writing component tests
- Testing hooks
- Testing utilities

**Key patterns**:
- Component testing
- Query priorities
- Async testing
- Mocking

---

#### `git-submodule-workflow`
Git submodule handling for `src/_core`

**When used**:
- Changes affecting submodule
- Understanding branch strategy
- Creating submodule PRs

**Key patterns**:
- Submodule vs parent changes
- Branch creation workflow
- Commit order (submodule first)

---

## Usage Examples

### Example 1: Full Workflow

```bash
# Start complete workflow
/workflow TAN-456
```

**What happens**:
1. Analyzes ticket TAN-456 from LINEAR
2. Explores codebase for relevant files
3. Creates implementation plan
4. Shows plan and asks for approval
5. After approval: implements features
6. Generates tests
7. Reviews code
8. Creates PR

---

### Example 2: Phased Approach

```bash
# Step 1: Analyze only
/analyze-ticket TAN-789

# Review the plan...

# Step 2: Implement (can run immediately or weeks later)
/implement TAN-789

# Step 3: Review local changes
/review

# Step 4: Create PR
/create-pr

# Step 5: Review PR
/review-pr 123
```

**Benefit**: More control at each step

---

### Example 3: Analyze Multiple Tickets

```bash
# Analyze to understand scope
/analyze-ticket TAN-100 --auto-write
/analyze-ticket TAN-101 --auto-write
/analyze-ticket TAN-102 --auto-write

# Decide which to implement (each is self-contained)
/implement TAN-101
```

---

## Best Practices

### When to Use Each Command

**Use `/workflow`**:
- Standard ticket implementation
- When you want full automation
- Trust the plan will be good

**Use `/analyze-ticket`**:
- Complex tickets needing review
- Want to review plan before coding
- Multiple tickets to evaluate

**Use individual commands**:
- Already have analysis in LINEAR
- Want granular control
- Working on ticket weeks after analysis
- Making iterative changes

---

### Skills Usage

Skills are **automatically used** by agents based on context. You don't invoke them directly.

**Example**: When `code-implementer` sees:
- File in `src/_core/` → Uses `git-submodule-workflow` skill
- React component → Uses `react-component-patterns` skill
- API call → Uses `crud-operations` skill
- Test file → Uses `frontend-testing` skill

---

## Adding New Platforms

To add support for new platforms (backoffice, backend):

1. **Create skill directory**:
   ```bash
   mkdir -p plugins/oneclick/skills/backend
   ```

2. **Add platform skills**:
   ```
   skills/backend/
   ├── api-design-patterns/SKILL.md
   ├── database-migrations/SKILL.md
   └── openapi-spec/SKILL.md
   ```

3. **Agents automatically use new skills** when working on that platform

No need to duplicate agents or commands!

---

## Troubleshooting

### Command not found
**Issue**: `/workflow` not recognized

**Solution**:
- Restart Claude Code after installation
- Run `/help` to verify commands loaded

---

### Agents not using skills
**Issue**: Agent doesn't follow patterns

**Solution**:
- Skills are model-invoked (automatic)
- Ensure skill SKILL.md has clear description
- Agent will use skill when context matches

---

### Todo list not created
**Issue**: `/implement` says no todos exist

**Solution**:
- Must run `/analyze-ticket` first
- Or use `/workflow` which includes analysis

---

## Support

- **Marketplace Issues**: https://github.com/Oneclickdys/ai-agents/issues
- **Claude Code Docs**: https://docs.claude.com/en/docs/claude-code

---

**Version**: 1.0.0
**Maintained By**: Oneclick Development Team
