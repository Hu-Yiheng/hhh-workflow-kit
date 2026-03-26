# CLAUDE.md

This file provides guidance to Claude Code when working with Java Spring Boot projects.

## Project Type

**Java Spring Boot Enterprise Application**

Technology Stack:
- Java 17+
- Spring Boot 3.x
- MyBatis
- Redis
- MySQL

## Core Workflow

Follow the 4-step workflow for all development tasks:

1. **Understand** - Clarify requirements and identify affected components
2. **Plan** - Create step-by-step implementation plan
3. **Implement** - Execute changes following the plan
4. **Verify** - Test and validate the implementation

## Rules

All rules are located in `.claude/rules/`:

- **workflow.md** - Core development workflow (understand → plan → implement → verify)
- **java-spring.md** - Java Spring Boot coding standards and best practices
- **team-guide.md** - Team collaboration guidelines, anti-patterns, and checklists

## Key Principles

- **Plan before coding** - Never start implementation without a clear plan
- **Minimal changes** - Keep changes as small as possible
- **Test everything** - Verify all changes work correctly
- **Follow standards** - Adhere to team coding conventions
- **Document decisions** - Record important choices and trade-offs

## Development Notes

- Package manager: Maven or Gradle
- Code style: Follow team conventions in `java-spring.md`
- Testing: Unit tests required for all business logic
- Security: Never hardcode secrets, always validate input
