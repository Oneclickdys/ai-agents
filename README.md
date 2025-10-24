# Oneclick AI Agents Marketplace

A comprehensive plugin marketplace for [Claude Code](https://github.com/anthropics/claude-code) featuring specialized agents, commands, and skills for LINEAR ticket implementation workflows across all Oneclick platforms.

> Built following [Claude Code plugin architecture](https://docs.claude.com/en/docs/claude-code/plugins) and [Anthropic's best practices](https://github.com/anthropics/claude-code/tree/main/plugins).

## Quick Start

### Installation

```bash
# Add the Oneclick marketplace
/plugin marketplace add Oneclickdys/ai-agents

# Install the Oneclick plugin
/plugin install oneclick@oneclick
```

### Usage

```bash
# Full workflow: Analysis → Implementation → PR → Review
/workflow TAN-123

# Or run phases independently:
/analyze-ticket TAN-123  # Phase 1: Analysis & Planning
/implement TAN-123       # Phase 2: Implementation & Testing
/review                  # Review local changes (diff with base branch)
/create-pr               # Create pull request
/review-pr [pr-number]   # Review the PR

# Utility commands:
/create-task             # Create LINEAR tasks
/update-task             # Update LINEAR tasks
```

---

## Architecture Philosophy

### Why One Plugin?

The Oneclick plugin contains all agents for LINEAR ticket implementation because they form a **cohesive workflow** working toward one goal. This follows Anthropic's best practices seen in `feature-dev` and `pr-review-toolkit` plugins.

### Organizing Principles

**Plugin Boundary** = Cohesive Workflow
- All ticket implementation agents in ONE plugin
- They're interdependent (implementation uses the plan from architecture)
- Users install one plugin to get complete workflow
- Avoids artificial separation of related functionality

**Agent Boundary** = Specialized Role
- Each agent has ONE clear focus area
- ticket-analyzer (parsing), codebase-explorer (understanding), code-architect (designing), etc.
- Can be invoked individually or orchestrated by commands
- Generic across all platforms (frontoffice, backoffice, backend)

**Skill Boundary** = Platform Knowledge
- Frontoffice skills (React, git submodules, frontend testing)
- Backoffice skills (admin patterns, RBAC) - *coming soon*
- Backend skills (API design, database migrations) - *coming soon*
- Agents autonomously use relevant skills based on context (model-invoked)

**This is NOT**:
- ❌ Three separate plugins (analyze-plugin, implement-plugin, review-plugin)
  - Would require installing multiple plugins for one workflow
  - Creates artificial separation of interdependent steps

- ❌ Platform-specific plugins (frontoffice-plugin, backend-plugin)
  - Would duplicate agent logic across platforms
  - Only skills differ between platforms

---

## What's Included

### 📦 Plugin: `oneclick`

**7 Specialized Agents**:
| Agent | Role | When Invoked |
|-------|------|--------------|
| ticket-analyzer | Parse LINEAR requirements | /analyze-ticket, /workflow |
| codebase-explorer | Understand project structure | /analyze-ticket, /workflow |
| code-architect | Design implementation approach | /analyze-ticket, /workflow |
| code-implementer | Write production code | /implement, /workflow |
| test-generator | Create comprehensive tests | /implement, /workflow |
| code-reviewer | Validate code quality | /review, /workflow |
| pr-creator | Create pull requests | /review, /workflow |

**8 Commands**:
- `/workflow [TICKET-ID]` - Full workflow (analyze → implement → create-pr → review-pr)
- `/analyze-ticket [TICKET-ID] [--auto-write]` - Analysis & planning phase
- `/implement [TICKET-ID] [--auto-commit]` - Implementation & testing phase
- `/review` - Review local changes (diff with base branch)
- `/create-pr` - Create pull request
- `/review-pr [pr-number]` - Review pull request
- `/create-task` - Create LINEAR tickets
- `/update-task [TICKET-ID]` - Update LINEAR tickets

**5 Frontoffice Skills**:
- crud-operations - CRUD patterns for API operations
- react-component-patterns - React component architecture
- global-state-management - Redux/state patterns
- frontend-testing - Jest & React Testing Library
- git-submodule-workflow - Git submodule handling

---

## How It Works

### Agent Orchestration

Commands invoke specialized agents based on the workflow phase:

```
/workflow TAN-123
    ↓
┌─────────────────────┐
│ Phase 1: Analysis   │
├─────────────────────┤
│ ticket-analyzer     │ ← Parse LINEAR ticket
│ codebase-explorer   │ ← Understand code
│ code-architect      │ ← Design approach
└─────────────────────┘
    ↓ (User approval)
┌─────────────────────┐
│ Phase 2: Implement  │
├─────────────────────┤
│ code-implementer    │ ← Write code
│ test-generator      │ ← Create tests
└─────────────────────┘
    ↓
┌─────────────────────┐
│ Phase 3: Review     │
├─────────────────────┤
│ code-reviewer       │ ← Validate quality
│ pr-creator          │ ← Submit PR
└─────────────────────┘
```

### Skills as Differentiators

Agents are **generic** and work across all platforms. **Skills** provide platform-specific knowledge:

```javascript
// code-implementer agent working on frontoffice code
// Autonomously uses frontoffice skills:

✓ Uses git-submodule-workflow skill (detects src/_core changes)
✓ Uses react-component-patterns skill (creating React components)
✓ Uses crud-operations skill (implementing API calls)
✓ Uses frontend-testing skill (writing Jest tests)
```

When working on backend code (future), the same `code-implementer` agent would use **backend skills** instead.

---

## Benefits

✅ **Follows Anthropic best practices** - Plugin organization like `feature-dev`, agent specialization like `pr-review-toolkit`

✅ **No duplication** - Single plugin with specialized generic agents

✅ **Clear boundaries** - Plugin (workflow) → Agent (role) → Skill (platform knowledge)

✅ **Scalable** - Add new platforms by adding skills, not duplicating agents

✅ **Composable** - Commands can invoke agents individually or as workflows

✅ **Maintainable** - Each component has clear, focused responsibility

✅ **Platform-agnostic agents** - Skills differentiate behavior across platforms

---

## Platform Support

### Current
- ✅ **Frontoffice** - Full skill support

### Coming Soon
- 🔄 **Backoffice** - Admin panel skills
- 🔄 **Backend** - API development skills

---

## Requirements

- [Claude Code](https://github.com/anthropics/claude-code) 1.0+ ([Installation guide](https://docs.claude.com/en/docs/claude-code))
- Git configured
- GitHub CLI (`gh`) for PR creation
- LINEAR MCP server (auto-configured during plugin installation)

### LINEAR MCP Configuration

The LINEAR MCP server is automatically configured when you install the plugin. If you need to configure it manually, add this to your Claude Code settings at `~/.claude/settings.json`:

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

---

## Contributing

We welcome contributions! To add new skills or agents:

1. Fork this repository
2. Create a feature branch
3. Make your changes:
   - New agents: Add to `plugins/oneclick/agents/`
   - New commands: Add to `plugins/oneclick/commands/`
   - New skills: Add to `plugins/oneclick/skills/{platform}/`
4. Follow existing patterns and naming conventions
5. Test with local marketplace
6. Submit pull request

---

## Support

- **Issues**: Open an issue in this repository
- **Documentation**: See `plugins/oneclick/README.md` for detailed plugin docs
- **Claude Code Docs**: https://docs.claude.com/en/docs/claude-code

---

## License

MIT License - See LICENSE file for details

**Maintained By**: Oneclick Development Team
**Last Updated**: 2025-10-24
