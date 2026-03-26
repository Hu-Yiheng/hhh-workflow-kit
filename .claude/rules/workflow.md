# Development Workflow

## Core Workflow: 4 Steps

Every development task follows this workflow:

### 1. Understand
- Clarify requirements completely
- Identify affected components
- List assumptions and constraints
- Ask questions if anything is unclear

### 2. Plan
- Create step-by-step implementation plan
- Identify dependencies and risks
- Break down into phases
- **WAIT for user confirmation before coding**

### 3. Implement
- Follow the plan strictly
- Keep changes minimal
- Mark any deviations from plan
- Use Test-Driven Development (TDD)

### 4. Verify
- Run all tests (unit + integration)
- Verify 80%+ test coverage
- Check for errors and warnings
- Validate against requirements

---

## Test-Driven Development (TDD)

**MANDATORY for all new features and bug fixes:**

1. **Write test first** (RED)
   - Test should fail initially

2. **Write minimal implementation** (GREEN)
   - Make the test pass

3. **Refactor** (IMPROVE)
   - Clean up code while keeping tests green

4. **Verify coverage** (80%+)
   - Check test coverage meets minimum

---

## Git Workflow

### Commit Message Format

```
<type>: <description>

<optional body>
```

**Types:** feat, fix, refactor, docs, test, chore, perf, ci

### Pull Request Process

1. Analyze full commit history (not just latest commit)
2. Use `git diff [base-branch]...HEAD` to see all changes
3. Draft comprehensive PR summary
4. Include test plan
5. Push with `-u` flag if new branch

---

## Key Principles

- ❌ **Never start coding without a plan**
- ❌ **Never skip tests**
- ✅ **Plan → Implement → Verify**
- ✅ **Write tests first (TDD)**
- ✅ **Keep changes minimal**
- ✅ **Get user confirmation before major changes**
