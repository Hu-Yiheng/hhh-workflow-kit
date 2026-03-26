# Team Collaboration Guide

## Anti-Patterns

### Dangerous Areas

These areas require extra caution when modifying:

- **Authentication/Authorization logic** - Security-critical
- **Payment processing** - Financial impact
- **Database migrations** - Data integrity risk
- **Shared utility functions** - Wide impact
- **Cache invalidation** - Consistency issues

**Rule: Always understand the full context before modifying these areas**

---

### Common Mistakes

#### 1. Modifying Shared Utils Without Checking Callers

❌ **Wrong:**
```java
// Directly modify shared utility
public class StringUtils {
    public static String format(String input) {
        return input.trim(); // Changed behavior!
    }
}
```

✅ **Correct:**
```java
// Search all callers first
// Use IDE "Find Usages" or grep
// Assess impact before changing
```

#### 2. Skipping Input Validation

❌ **Wrong:**
```java
@PostMapping
public Result<Void> createUser(@RequestBody UserDTO dto) {
    userService.createUser(dto); // No validation!
}
```

✅ **Correct:**
```java
@PostMapping
public Result<Void> createUser(@Valid @RequestBody UserDTO dto) {
    userService.createUser(dto);
}
```

#### 3. Silent Scope Expansion

❌ **Wrong:**
```
Task: Fix login bug
Changes: Fixed login + refactored auth + updated UI
```

✅ **Correct:**
```
Task: Fix login bug
Changes: Only fixed the login bug
Note: Found refactoring opportunity - created new task
```

#### 4. No Tests = Not Done

❌ **Wrong:**
```
"Code is written, task complete!"
(No tests, not verified)
```

✅ **Correct:**
```
"Code written + tests passing + coverage 80%+ = complete"
```

#### 5. Hardcoded Secrets

❌ **Wrong:**
```java
String apiKey = "sk-proj-xxxxx";
String dbPassword = "admin123";
```

✅ **Correct:**
```java
@Value("${api.key}")
private String apiKey;

@Value("${spring.datasource.password}")
private String dbPassword;
```

---

## Must-Confirm Changes

These changes require human confirmation before proceeding:

### 1. Schema Changes
- Database table structure modifications
- API field additions/deletions
- Configuration file format changes

### 2. API Contract Changes
- Interface signature modifications
- Response format changes
- Error code adjustments

### 3. Dependency Upgrades
- Major version upgrades
- Breaking changes in dependencies

### 4. Security-Related
- Authentication/authorization logic
- Encryption algorithm changes
- Sensitive data handling

### 5. Performance-Critical Paths
- Database query optimization
- Cache strategy adjustments
- Concurrency control modifications

---

## Code Review Checklist

### Before Submitting PR

- [ ] All tests passing
- [ ] Test coverage ≥ 80%
- [ ] No hardcoded secrets
- [ ] Input validation in place
- [ ] Error handling complete
- [ ] Logging added for key operations
- [ ] No `System.out.println` or debug code
- [ ] Code follows team conventions
- [ ] Commit messages are clear
- [ ] PR description explains changes

### Reviewer Checklist

#### Functionality
- [ ] Does it solve the stated problem?
- [ ] Are edge cases handled?
- [ ] Is error handling appropriate?

#### Code Quality
- [ ] Is the code readable?
- [ ] Are names clear and descriptive?
- [ ] Is complexity reasonable?
- [ ] Are there code smells?

#### Security
- [ ] Is input validated?
- [ ] Are there SQL injection risks?
- [ ] Are secrets properly managed?
- [ ] Is authentication/authorization correct?

#### Performance
- [ ] Are there N+1 query issues?
- [ ] Is caching appropriate?
- [ ] Are there memory leaks?
- [ ] Is concurrency handled correctly?

#### Testing
- [ ] Are tests comprehensive?
- [ ] Do tests actually test the logic?
- [ ] Is coverage adequate?
- [ ] Are integration tests included?

---

## Communication Guidelines

### When to Ask for Help

- Unclear requirements
- Blocked by dependencies
- Encountering unexpected complexity
- Security concerns
- Performance issues

### How to Report Issues

```markdown
## Issue Description
[Clear description of the problem]

## Steps to Reproduce
1. Step 1
2. Step 2
3. Step 3

## Expected Behavior
[What should happen]

## Actual Behavior
[What actually happens]

## Environment
- Java version:
- Spring Boot version:
- Database:

## Logs/Screenshots
[Relevant logs or screenshots]
```

---

## Best Practices

### Do's ✅

- Plan before coding
- Write tests first (TDD)
- Keep changes small and focused
- Commit frequently with clear messages
- Ask questions when unclear
- Document important decisions
- Review your own code before submitting
- Learn from code reviews

### Don'ts ❌

- Don't skip planning
- Don't skip tests
- Don't expand scope silently
- Don't commit commented-out code
- Don't ignore warnings
- Don't hardcode configuration
- Don't merge without review
- Don't take shortcuts on security

---

## Emergency Procedures

### Production Issue

1. **Assess severity** - Is it critical?
2. **Notify team** - Alert relevant people
3. **Investigate** - Check logs, metrics
4. **Fix or rollback** - Choose safest option
5. **Document** - Record what happened
6. **Post-mortem** - Learn and improve

### Rollback Procedure

```bash
# 1. Identify last good version
git log --oneline

# 2. Create rollback branch
git checkout -b rollback/issue-description

# 3. Revert problematic commits
git revert <commit-hash>

# 4. Test thoroughly
mvn clean test

# 5. Deploy
# Follow deployment procedure
```

---

## Knowledge Sharing

### Document These

- Architecture decisions
- Complex business logic
- Workarounds and their reasons
- Performance optimizations
- Security considerations
- Integration points

### Where to Document

- Code comments (for complex logic)
- README files (for setup and usage)
- Wiki/Confluence (for architecture)
- ADRs (Architecture Decision Records)
- Inline documentation (for APIs)
