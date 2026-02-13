---
title: feat Create Go Style Guide Plugin
slug: go-style-guide-plugin
date: 2026-02-12
category: plugin-development
component: go-style-guide-plugin
tags: [go, style-guide, marketplace, code-review, testing, golang-principle, golang-testing, go-reviewer]
---

# Go Style Guide Plugin Implementation

## Problem

The user required creation of a comprehensive Go style guide plugin to codify Google Go Style Guide principles for use within Claude Code. The requirements specified:

- A `golang-principle` skill for Go code compliance (writing, refactoring, code review)
- A `golang-testing` skill for testing-specific guidelines
- A `go-reviewer` agent for automated code review
- Language-agnostic coding principles as reusable rules

### Why This Mattered

- **Standardization**: Provide consistent Go coding standards across projects
- **Best Practices**: Codify Google's proven Go style decisions and best practices
- **Code Quality**: Improve Go code readability, maintainability, and consistency
- **Agent Support**: Enable automated code review with Go-specific guidelines
- **Knowledge Capture**: Language-agnostic principles for broader application

## Investigation Steps

Research was conducted across the following sources:

1. **Google Go Style Guide** (https://google.github.io/styleguide/go/)
   - Core principles: Clarity, Simplicity, Concision, Maintainability, Consistency
   - Detailed style choices with rationale
   - Common patterns and guidance

2. **Google Python Style Guide** (https://google.github.io/styleguide/pyguide)
   - Cross-language principles validated
   - Code clarity and maintainability patterns

3. **Effective Go** (https://go.dev/doc/effective_go)
   - Official Go idioms and conventions
   - Package naming, interfaces, testing basics

4. **Go Tutorial: Testing** (https://go.dev/doc/tutorial/add-a-test)
   - Test structure and conventions
   - Test naming patterns

## Solution

### File Structure Created

```
plugins/go-style-guide/
├── .claude-plugin/
│   └── plugin.json              # Plugin metadata
├── README.md                     # Installation and usage
├── agents/
│   └── go-reviewer/
│       └── AGENT.md              # Go code review agent
└── skills/
    ├── golang-principle/
    │   └── SKILL.md              # Go coding principles
    └── golang-testing/
        └── SKILL.md              # Go testing guidelines

.claude-plugin/
└── marketplace.json              # Marketplace configuration

rules/common/
└── coding-principles.md          # Language-agnostic principles
```

### Plugin Metadata

**`.claude-plugin/plugin.json`**:
```json
{
  "name": "go-style-guide",
  "description": "Use when writing, refactoring, or reviewing Go code. Provides idiomatic Go coding standards based on Google's style guide, including naming conventions, error handling, testing patterns, and best practices.",
  "version": "1.0.0",
  "author": {
    "name": "zoumo",
    "email": "jim.zoumo@gmail.com"
  }
}
```

**`marketplace.json`** (added to plugins array):
```json
{
  "name": "go-style-guide",
  "source": "./go-style-guide",
  "description": "Go style guide with coding standards, best practices and testing patterns",
  "version": "1.0.0",
  "author": {
    "name": "zoumo",
    "email": "jim.zoumo@gmail.com"
  },
  "category": "language-guide",
  "keywords": ["go", "golang", "style", "code-review", "testing"]
}
```

### Key Code Examples

**Table-Driven Test Pattern** (from `golang-testing/SKILL.md`):
```go
func TestCalculateSum(t *testing.T) {
    tests := []struct {
        name    string
        input   []int
        want    int
    }{
        {"empty slice", nil, 0},
        {"single element", []int{5}, 5},
        {"multiple elements", []int{1, 2, 3}, 6},
        {"negative numbers", []int{-1, -2, 3}, 0},
    }

    for _, tt := range tests {
        t.Run(tt.name, func(t *testing.T) {
            assert.Equal(t, tt.want, CalculateSum(tt.input))
        })
    }
}
```

**Error Wrapping with Security** (from `golang-principle/SKILL.md`):
```go
// Good - constant query with parameterization
const getUserQuery = `SELECT * FROM users WHERE id = $1`
if err := db.QueryRow(getUserQuery, id).Scan(&user); err != nil {
    return fmt.Errorf("failed to query user: %w", err)
}
```

**Test Helper with t.Helper()** (from `golang-testing/SKILL.md`):
```go
func assertUser(t *testing.T, got, want *User) {
    t.Helper() // marks this as a helper - errors point to caller

    assert.Equal(t, want.Name, got.Name)
    assert.Equal(t, want.Email, got.Email)
}

func TestUserClone(t *testing.T) {
    original := &User{Name: "Alice", Email: "alice@example.com"}
    clone := original.Clone()

    assertUser(t, clone, original)  // this line is shown on error
}
```

### Code Review Findings Fixed

| Finding | Description | Fix Applied |
|---------|-------------|-------------|
| P2-1 | Missing security guidance | Added "Security-Aware Error Messages" section distinguishing internal vs external errors |
| P2-2 | SQL injection risk | Updated to use constant query with parameterization, removed user ID from error logging |
| P3-4 | Duplicate content | Removed "Common Mistakes" tables from both SKILL.md files |
| P3-5 | Redundant section | Removed "Common Violations" section from AGENT.md |
| P3-6 | Generic guidance | Removed Kubernetes style guide requirement, follow standard Go conventions only |

**Note**: P2-3 (comprehensive security guidance) was waived per user request.

## Related Patterns

The go-style-guide follows the `chinese-style-guide` plugin pattern:

| Pattern Element | chinese-style-guide | go-style-guide |
|-----------------|---------------------|----------------|
| Directory structure | `plugins/{name}/.claude-plugin/plugin.json` | Same |
| Skills location | `skills/{name}/SKILL.md` | Same |
| YAML frontmatter | `name` + `description` | Same |
| Quick reference table | Good/Avoid columns | Same |
| Code examples | Good/Avoid markdown blocks | Same |
| README structure | Installation via marketplace | Same |
| Agent directory | None | Adds `agents/` for `go-reviewer` |

**Key additions in go-style-guide**:
- `agents/` directory for `go-reviewer` agent
- Two skills (`golang-principle` and `golang-testing`)
- Language-agnostic shared rules (`rules/common/coding-principles.md`)

## Prevention Strategies

### Plugin Development Best Practices

**YAML Frontmatter Standards**:
```yaml
---
name: <lowercase-with-hyphens>
description: Use when [specific condition]. [summary of what it does].
---
```

**Content Organization Order**:
1. Overview
2. When to Use
3. Quick Reference (table)
4. Detailed Rules (grouped by category)
5. Code Examples
6. Common Idioms
7. References

### Common Pitfalls to Avoid

| Pitfall | Problem | Prevention |
|---------|---------|------------|
| YAML frontmatter errors | Extra spaces, incorrect hyphen count | Use exactly `---`, only `name` and `description` |
| Conflicting guidance | Style guide conflicts with linters | Document tool-specific behavior explicitly |
| Trigger ambiguity | Overlapping skill descriptions | Make distinctions explicit in descriptions |
| Outdated references | Style guides change, link rot | Reference official stable URLs |
| Security misguidance | Insecure code examples | Always show secure alternatives |
| Marketplace inconsistency | Different version numbers | Single source of truth for version |

### Maintenance Considerations

- **Monitor Sources**: Subscribe to Google Go Style Guide RSS/notifications
- **Version Tracking**: Document Go version targeted, note version-specific rules
- **Semantic Versioning**: PATCH (docs/typos), MINOR (new rules), MAJOR (breaking changes)

## Testing Recommendations

1. **Syntax Validation**: Verify YAML frontmatter parses, check all code blocks
2. **Example Compilation**: Ensure all Go examples compile (not just run)
3. **Integration Testing**: Test skill invocation, agent activation, marketplace installation
4. **Usecase Testing**: Test against sample Go projects, verify suggestions are actionable

## References

- [Google Go Style Guide](https://github.com/google/styleguide/tree/main/go)
- [Google Go Style Guide (web)](https://google.github.io/styleguide/go/)
- [Effective Go](https://go.dev/doc/effective_go)
- [Go Tutorial - Testing](https://go.dev/doc/tutorial/add-a-test)
- [Go Code Review Comments](https://github.com/golang/go/wiki/CodeReviewComments)
- [Testify](https://github.com/stretchr/testify)
- [Repository: github.com/zoumo/dotclaude](https://github.com/zoumo/dotclaude)