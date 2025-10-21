---
argument-hint: [TANAI-XXXX]
description: Execute agent workflow to implement a Linear task in tangerine-ai (3 stages)
allowed-tools: Task, Grep, Glob, Read, Edit, MultiEdit, Write, Bash, TodoWrite, TodoRead, git, mcp__linear__*, mcp__ide__*
response-language: es
---

I will execute a 3-stage workflow to implement the Linear task: **$ARGUMENTS**

CRITICAL: perform environment setup before starting the stages.

## Repository context

- Backend project: `tangerine-ai` (AI API)
- Related frontend repo: `tangerine-frontoffice`
- Default branch: `main`
- No submodules

## Environment setup

1. Retrieve Linear task details (MCP)
2. `git fetch origin`
3. `git checkout main && git pull`
4. Create working branch from `main`: `git checkout -b tanai-XXXX-description`
5. Install dependencies if needed: `npm ci`
6. Configure local environment variables if required

## Stages

### Stage 1: Analysis & planning

- Parse acceptance criteria and specs
- Identify scope (backend API only; cross-impact to `tangerine-frontoffice` if any)
- Analyze code to locate affected services/modules/infrastructure
- Design approach aligned with architecture (DDD/Clean Architecture)
- Create detailed todo list with `TodoWrite`
- Flag external/data dependencies (Prisma, queues, etc.)

🛑 Checkpoint: present plan and todo list for user validation

### Stage 2: Implementation & testing

- Implement according to the todo list
- Add tests (unit/integration) as appropriate
- Run linter and tests: `npm run lint`, `npm test`
- Create commits with `/commit` (prefix `TANAI-XXXX`)

### Stage 3: Review & Pull Request

- Run final checks (lint/tests)
- Push branch and create PR with `/create-mr` (base `main`)
- Include technical summary, validations and `TANAI-XXXX` reference
- Return PR URL

## Iterative process

- Accept feedback, adjust plan, add commits and re-run validations

**Starting workflow for task: $ARGUMENTS**

Beginning by launching the Analysis Agent to fetch and analyze Linear task details.
