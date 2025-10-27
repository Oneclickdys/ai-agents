# Tangerine Frontoffice - Development Guidelines

This document contains all the development guidelines and rules for working on the Tangerine Frontoffice project.

## Project Rules and Guidelines

The following files contain detailed guidelines for different aspects of development:

@.cursor/rules/general.mdc
@.cursor/rules/core-components.mdc
@.cursor/rules/crud.mdc
@.cursor/rules/styles.mdc
@.cursor/rules/global-state-management.mdc
@.cursor/rules/unit-testing.mdc
@.cursor/rules/cursor-rules.mdc

## Quick Reference

- **Tech Stack**: React 17, Redux + Redux Toolkit, React Router v5, SASS
- **Testing**: Jest + React Testing Library
- **Code Quality**: ESLint + Prettier
- **Architecture**: Modular architecture with Core/App separation

## Important Notes

- Always follow the established code conventions
- Document components and functions
- Write tests for new functionality
- Maintain the modular architecture
- Always ask if you have doubts or uncertainties
- Follow the user's requirements carefully & to the letter
- Confirm, then write code!

## Quick Visual Check

IMMEDIATELY after implementing any front-end change:

Identify what changed – Review the modified components/pages

Navigate to affected pages – Use `mcp__playwright__browser_navigate` to visit each changed view

Verify design compliance – Compare against .cursor/rules/styles.mdc and .cursor/rules/core-components.mdc

Validate feature implementation – Ensure the change fulfills the user's specific request

Check acceptance criteria – Review any provided context files or requirements

Capture evidence – Take full page screenshot at desktop viewport (1440px) of each changed view

Check for errors – Run `mcp__playwright__browser_console_messages`
