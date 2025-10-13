# AI Agents - Claude Code Configuration

A comprehensive configuration repository for Claude Code with custom agents, slash commands, and workflows designed to streamline development processes.

## Overview

This repository contains a production-ready setup for Claude Code that includes:

- Custom AI agents for analysis, planning, implementation, and code review
- Slash commands for common development workflows
- Integration with LINEAR project management
- Git workflow automation
- Repository architecture documentation

## Features

### Agentic Workflows

Three specialized AI agents work together to automate development tasks:

1. **Analysis & Planning Agent** (`agents/analysis-planning-agent.md`)
   - Analyzes task requirements from LINEAR
   - Creates comprehensive implementation plans
   - Identifies dependencies and potential issues

2. **Implementation Agent** (`agents/implementation-agent.md`)
   - Executes the implementation plan
   - Writes code following best practices
   - Runs tests and ensures quality

3. **Review Agent** (`agents/review-agent.md`)
   - Reviews code changes
   - Creates pull requests with detailed descriptions
   - Ensures proper documentation

### Slash Commands

#### Primary Workflow Commands

- `/start-work [TASK-ID]` - Complete workflow from task analysis to PR creation
- `/commit` - Create git commits with proper references
- `/create-mr` - Create GitHub pull requests

#### Task Management Commands

- `/create-task` - Create new LINEAR tasks with AI-enhanced descriptions
- `/update-task [TASK-ID]` - Update existing LINEAR tasks with codebase analysis

## Installation

### Option 1: Clone to Your Project

```bash
# Clone this repository into your project's .claude directory
git clone https://github.com/Oneclickdys/ai-agents.git .claude
```

### Option 2: Copy Configuration Files

```bash
# Copy the entire structure to your project
cp -r ai-agents/ your-project/.claude/
```

### Configuration

1. **Update Repository-Specific Information:**
   - Edit paths in documentation files to match your project structure
   - Update repository URLs and LINEAR workspace references
   - Customize task templates for your workflow

2. **Set Up LINEAR Integration:**
   - Ensure you have LINEAR CLI or API access configured
   - Update LINEAR workspace ID in command files

3. **Configure Git Settings:**
   - Verify GitHub CLI (`gh`) is installed and authenticated
   - Adjust branch naming conventions if needed

## Directory Structure

```
.claude/
├── README.md                        # This file
├── REPOSITORY-STRUCTURE.md          # Repository architecture guide
├── BACKEND-FRONTEND-GUIDE.md        # Backend vs Frontend decision guide
├── commands/
│   ├── start-work.md                # Main agentic workflow
│   ├── commit.md                    # Git commit creation
│   ├── create-mr.md                 # Pull request creation
│   ├── create-task.md               # LINEAR task creation
│   └── update-task.md               # LINEAR task updates
├── agents/
│   ├── analysis-planning-agent.md   # Stage 1: Analysis & Planning
│   ├── implementation-agent.md      # Stage 2: Implementation
│   └── review-agent.md              # Stage 3: Review & PR
└── task-template.md                 # LINEAR task template
```

## Usage

### Quick Start

1. **Start working on a task:**
   ```bash
   /start-work TASK-123
   ```

2. **The system will automatically:**
   - Fetch task details from LINEAR
   - Create appropriate branches
   - Analyze requirements and create a plan
   - Implement the changes
   - Run tests
   - Create commits
   - Generate pull request(s)

### Manual Workflow

If you prefer more control:

1. Make your changes manually
2. Create commits: `/commit`
3. Create PR: `/create-mr`

## Customization

### Adapting to Your Project

1. **Repository Structure:**
   - Update `REPOSITORY-STRUCTURE.md` with your repo architecture
   - Modify branch naming conventions in command files
   - Adjust base branches for PRs

2. **Task Management:**
   - Replace LINEAR with your project management tool (Jira, GitHub Issues, etc.)
   - Update task ID patterns in commands
   - Customize task templates

3. **Agents:**
   - Modify agent prompts to match your coding standards
   - Add custom validation steps
   - Include project-specific best practices

### Adding New Commands

Create a new markdown file in the `commands/` directory:

```markdown
---
description: Brief description of your command
---

Your command prompt here...
```

Access it via: `/your-command-name`

## Best Practices

1. Always start with `/start-work` for comprehensive task handling
2. Review agent-generated plans before implementation
3. Ensure all tests pass before creating PRs
4. Keep documentation files updated with your project specifics

## Requirements

- [Claude Code](https://docs.claude.com/en/docs/claude-code) installed
- Git configured
- GitHub CLI (`gh`) for PR creation
- LINEAR CLI or API access (optional, for task management)

## Contributing

Contributions are welcome! To contribute:

1. Fork this repository
2. Create a feature branch
3. Make your changes
4. Submit a pull request with a clear description

## Support

For issues or questions:
- Check the [Claude Code Documentation](https://docs.claude.com/en/docs/claude-code)
- Open an issue in this repository
- Contact the Oneclick development team

## License

This configuration is provided as-is for use within development projects.

---

**Maintained By**: Oneclick Development Team
**Last Updated**: 2025-10-13
