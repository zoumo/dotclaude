---
title: feat Create Go Style Guide Plugin
type: feat
date: 2026-02-12
---

# feat Create Go Style Guide Plugin

## Overview

Create a comprehensive Go style guide plugin that codifies the [Google Go Style Guide](https://github.com/google/styleguide) principles, decisions, and best practices for use within Claude Code. This plugin will provide a `golang-principle` skill for Go code compliance (writing, refactoring, code review) and a separate `golang-testing` skill for testing-specific guidelines, plus a `go-reviewer` agent for code review.

## Motivation

- **Standardization**: Provide consistent Go coding standards across projects
- **Best Practices**: Codify Google's proven Go style decisions and best practices
- **Code Quality**: Improve Go code readability, maintainability, and consistency
- **Agent Support**: Enable automated code review with Go-specific guidelines
- **Knowledge Capture**: Language-agnostic principles for broader application

## Research Findings

### Sources Consulted

1. **Google Go Style Guide** (https://google.github.io/styleguide/go/)
   - Guide: Core principles (Clarity, Simplicity, Concision, Maintainability, Consistency)
   - Decisions: Detailed style choices with rationale
   - Best Practices: Common patterns and guidance

2. **Google Python Style Guide** (https://google.github.io/styleguide/pyguide)
   - Cross-language principles validated
   - Code clarity, maintainability patterns

3. **Effective Go** (https://go.dev/doc/effective_go)
   - Official Go idioms and conventions
   - Package naming, interfaces, testing basics

4. **Go Tutorial: Testing** (https://go.dev/doc/tutorial/add-a-test)
   - Test structure and conventions
   - Test naming patterns

### Enhanced Language-Agnostic Principles

From analysis across Google's style guides and general software engineering principles, these are language-agnostic principles for `rules/common/coding-principles.md`:

1. **Clarity**
   - Code must explain "what" and "why"
   - Descriptive variable names over brevity
   - Clarity trumps cleverness (from Python guide)
   - Write code as readable as narrative

2. **Simplicity**
   - Prefer standard tools over custom solutions
   - Least mechanism principle
   - Top-to-bottom readability
   - Avoid unnecessary abstraction levels

3. **Concision**
   - High signal-to-noise ratio
   - Avoid redundant comments (don't repeat code)
   - Use common idioms
   - Boost important signals with contextual comments

4. **Maintainability**
   - Code is modified more than written
   - Easy to modify correctly
   - Clear assumptions
   - No hidden critical details
   - Predictable naming patterns
   - Minimize dependencies

5. **Consistency**
   - Follow existing patterns in codebase
   - Local consistency acceptable when not harming readability
   - Style deviations not acceptable if they worsen existing issues

6. **DRY (Don't Repeat Yourself)**
   - Avoid duplicate logic
   - Use abstraction when complexity justifies it

7. **KISS (Keep It Simple, Stupid)**
   - Prefer simple solutions
   - Avoid premature optimization

### Go-Specific Key Areas

**Formatting & Style**
- Use `gofmt` for all source files
- No fixed line length - refactor instead of splitting lines
- MixedCaps naming (camel case), no underscores (except tests/generated code)
- Package names: lowercase, single-word, no underscores

**Naming Conventions** (Enhanced from Effective Go)
- Variables/functions: `mixedCaps` for unexported, `MixedCaps` for exported
- Package names: lowercase, single-word (e.g., `bytes`, `fmt`, `http`)
- Getter methods: no "Get" prefix (e.g., `obj.Owner()` not `obj.GetOwner()`)
- Setter methods: `Set` + field name
- Interfaces: single-method interfaces end with `-er` suffix (e.g., `Reader`, `Writer`)
- Constants: use `MixedCaps`, `iota` for enumerations
- Receiver names: short abbreviations (e.g., `t Tray` not `tray Tray`)

**Error Handling**
- Check errors explicitly, never ignore with `_`
- Return `error` as last parameter
- Use `fmt.Errorf()` for error strings
- Error strings: no capitalization (unless proper noun/acronym), no punctuation
- Use `%w` for wrapping to preserve error chains
- Use `errors.Is()` for checking wrapped errors
- Don't panic for normal errors; use error returns

**Testing Patterns** (Enhanced from Go Tutorial)
- Table-driven tests for similar inputs
- Test functions: `Test<FunctionName>` or `Test<FunctionName>_<Case>`
- Test files: `*_test.go` in same package
- Use `t.Helper()` in test helpers
- Clear error messages: `Func(%v) = %v, want %v`
- User preference: `testify/assert` for assertions
- Skip assertion libraries unless compatible with testify

**API Design**
- Accept interfaces, return concrete types
- Pass `context.Context` as first parameter in methods
- Avoid generic package names like `util`, `helper`
- Prefer small, focused packages
- Document exported APIs with clear godoc formatting

**Code Organization**
- Small, focused functions with single responsibilities
- Consistent indentation and formatting
- Organize imports in groups

## Implementation Plan

### Phase 1: Language-Agnostic Rules

**Deliverable**: Create `rules/common/coding-principles.md` (language-agnostic coding principles)

Create `rules/common` subdirectory and write the file with:

```markdown
# Language-Agnostic Coding Principles

## Clarity
- Code must explain "what" and "why"
- Descriptive variable names over brevity
- Clarity trumps cleverness
- Write code as readable as narrative

## Simplicity
- Prefer standard tools over custom solutions
- Least mechanism principle
- Top-to-bottom readability
- Avoid unnecessary abstraction levels

## Concision
- High signal-to-noise ratio
- Avoid redundant comments
- Use common idioms
- Boost important signals with comments

## Maintainability
- Code is modified more than written
- Easy to modify correctly
- Clear assumptions
- No hidden critical details
- Predictable naming patterns
- Minimize dependencies

## Consistency
- Follow existing patterns in codebase
- Local consistency acceptable when not harming readability
- Style deviations not acceptable if they worsen existing issues

## DRY (Don't Repeat Yourself)
- Avoid duplicate logic
- Use abstraction when complexity justifies it

## KISS (Keep It Simple, Stupid)
- Prefer simple solutions
- Avoid premature optimization
```

### Phase 2: Go Style Guide Plugin Structure

**Deliverables**: Plugin directory and metadata

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
```

### Phase 3: Plugin Metadata

**Deliverable**: `plugins/go-style-guide/.claude-plugin/plugin.json`

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

### Phase 4: Go Principles Skill

**Deliverable**: `plugins/go-style-guide/skills/golang-principle/SKILL.md`

Content structure following `format-chinese` style:

```yaml
---
name: golang-principle
description: Use when writing, refactoring, or reviewing Go code. Ensures idiomatic Go coding standards including naming conventions, error handling, API design, and code organization.
---
```

Sections to include:

- **Quick Reference Table** with Good/Avoid examples
  | Rule | Good | Avoid |
  |------|------|-------|
  | Package name | `package bytes` | `package byte_utils` |
  | Exported name | `func Parse(input)` | `func parse(input)` for public |
  | Unexported name | `func process()` | `func Process()` for private |
  | Getter no "Get" | `obj.Owner()` | `obj.GetOwner()` |
  | Interface name | `type Reader interface` | `type IReader interface` |
  | Receiver name | `func (c *Counter) Add()` | `func (counter *Counter) Add()` |
  | Error wrapping | `fmt.Errorf("context: %w", err)` | `fmt.Errorf("context: %v", err)` |
  | Error messages | `fmt.Errorf("invalid user ID: %d", id)` | `fmt.Errorf("Invalid user ID: %d.", id)` |
  | No panic for errors | `return fmt.Errorf("not found")` | `panic("not found")` |
  | Context first | `func Fetch(ctx, id)` | `func Fetch(id, ctx)` |
  | Null slice | `var items []Item` | `items := []Item{}` for local |
  | nil check | `if s != nil` | `if len(s) > 0` for nil check |

- **Naming Conventions**
  - Package names: lowercase, single-word, no underscores
    ```go
    // Good
    package bytes
    package http
    package tabwriter
    // Avoid
    package byte_util
    package http_client
    package TabWriter
    ```
  - MixedCaps for exported, mixedCaps for unexported
    ```go
    type User struct { ... }           // exported
    type userCache struct { ... }       // unexported
    func Process() {}                  // exported
    func processCache() {}              // unexported
    ```
  - No "Get" prefix for getters
    ```go
    // Good
    func (u *User) Name() string
    func (u *User) Age() int
    // Avoid
    func (u *User) GetName() string
    func (u *User) GetAge() int
    ```
  - Single-method interfaces: -er suffix
    ```go
    // Good
    type Reader interface {
        Read(p []byte) (n int, err error)
    }
    type Writer interface {
        Write(p []byte) (n int, err error)
    }
    // Avoid
    type IRead interface { ... }
    type ReadInterface interface { ... }
    ```
  - Constants: use MixedCaps with iota for enumerations
    ```go
    const (
        Read  = 1 << iota  // 1
        Write              // 2
        Exec               // 4
        Private            // 8
    )
    ```
  - Receiver names: short abbreviations, consistent across methods
    ```go
    // Good
    func (b *Buffer) Read(p []byte) (n int, err error) { ... }
    func (b *Buffer) Write(p []byte) (n int, err error) { ... }
    func (t *Tray) Add(item string) { ... }
    // Avoid
    func (buffer *Buffer) Read(p []byte) (n int, err error) { ... }
    func (buf *Buffer) Write(p []byte) (n int, err error) { ... }
    ```

- **Error Handling**
  - Always check errors, never ignore with `_`
    ```go
    // Good
    f, err := os.Open("file.txt")
    if err != nil {
        return fmt.Errorf("failed to open file: %w", err)
    }
    defer f.Close()
    // Avoid
    f, _ := os.Open("file.txt")
    defer f.Close()
    ```
  - Return error as last parameter
    ```go
    // Good
    func Parse(input string) (*Config, error)
    func Save(ctx context.Context, data []byte) error
    // Avoid
    func (error, *Config) Parse(input string)
    func Save(data []byte, ctx context.Context) error
    ```
  - Clear error messages: no capitalization, no punctuation
    ```go
    // Good
    return fmt.Errorf("invalid user ID: %d", id)
    return fmt.Errorf("context canceled while waiting for response")
    // Avoid
    return fmt.Errorf("Invalid user ID: %d.", id)
    return fmt.Errorf("Validation error: invalid input.")
    ```
  - Proper wrapping with %w to preserve error chains
    ```go
    // Good
    if err := db.QueryRow(query, id).Scan(&user); err != nil {
        return fmt.Errorf("failed to query user %d: %w", id, err)
    }
    // Use errors.Is to check wrapped errors
    if errors.Is(err, sql.ErrNoRows) {
        return ErrNotFound
    }
    ```
  - Don't panic for normal errors
    ```go
    // Good
    if id < 0 {
        return fmt.Errorf("invalid negative ID: %d", id)
    }
    // Avoid for normal errors
    if id < 0 {
        panic("invalid negative ID")
    }
    ```

- **API Design**
  - Context first parameter for methods that may block
    ```go
    // Good
    func Fetch(ctx context.Context, id int) (*User, error)
    func Process(ctx context.Context, data []byte) error
    // Avoid
    func Fetch(id int, ctx context.Context) (*User, error)
    ```
  - Accept interfaces, return concrete types
    ```go
    // Good
    func Write(w io.Writer, data []byte) error
    func Read(r io.Reader, buf []byte) (int, error)
    // Avoid (usually)
    func Write() (io.Writer, error)
    ```
  - Avoid generic package names
    ```go
    // Good
    package user
    package auth
    package config
    // Avoid
    package util
    package helper
    package common
    package utl
    ```
  - Proper godoc formatting
    ```go
    // Good - starts with function name, full sentence
    // Parse parses the input string and returns a Config.
    // It returns an error if the input is malformed.
    func Parse(input string) (*Config, error) { ... }
    // Avoid
    // This function parses the input.
    func Parse(input string) (*Config, error) { ... }
    ```

- **Code Organization**
  - gofmt formatting (always run gofmt)
  - Imports grouped in order: stdlib, third-party, project
    ```go
    import (
        // stdlib first
        "fmt"
        "os"

        // third-party
        "github.com/stretchr/testify/assert"

        // project imports last
        "myproject/internal/user"
    )
    ```
  - Small, focused functions (single responsibility)

- **Common Idioms**
  - `:=` for assignment where possible
    ```go
    // Good
    name, err := getName()
    count := len(items)
    // Avoid when variable already exists
    if name, err := getName(); err != nil {
        return err  // name shadowed in this scope
    }
    ```
  - if err := fn(); err != nil pattern
    ```go
    // Good - common idiom for error checking
    if err := validate(input); err != nil {
        return err
    }
    ```

Use `skill-creator` for creation.

### Phase 5: Go Testing Skill

**Deliverable**: `plugins/go-style-guide/skills/golang-testing/SKILL.md`

Content structure:

```yaml
---
name: golang-testing
description: Use when writing or reviewing Go tests. Covers table-driven tests, testify usage, test helpers, and proper test organization.
---
```

Sections to include:

- **Quick Reference Table**
  | Rule | Good | Avoid |
  |------|------|-------|
  | Test naming | `TestCalculateSum_ValidInput` | `testSum_valid` |
  | Test file | `calculator_test.go` | `calculator_tests.go` |
  | Test helpers | Use `t.Helper()` | Direct `t.Errorf` in helper |
  | Failure messages | `got %v, want %v` | "test failed" |
  | Assert parameter order | `assert.Equal(t, want, got)` | `assert.Equal(t, got, want)` |
  | Table tests | Define test cases struct | Repeating test code |

- **Test Organization**
  - File naming: `*_test.go`
    ```go
    // Good - file name matches source file
    user.go
    user_test.go
    // Avoid
    user.go
    user_tests.go
    userTesting.go
    ```
  - Function naming: `Test<FunctionName>` or `Test<FunctionName>_<Case>`
    ```go
    // Good
    func TestCalculateSum_EmptySlice(t *testing.T) { ... }
    func TestCalculateSum_SingleElement(t *testing.T) { ... }
    // Avoid
    func testSum(t *testing.T) { ... }
    func Test_sum(t *testing.T) { ... }
    func TestCalculatesum(t *testing.T) { ... }
    ```

- **Test Framework** - Use `testify/assert` for assertions
  ```go
  import (
      "github.com/stretchr/testify/assert"
      "github.com/stretchr/testify/require"
  )

  func TestUser(t *testing.T) {
      user := &User{Name: "Alice"}

      // assert - continues after failure
      assert.Equal(t, "Alice", user.Name)
      assert.NotNil(t, user.Email)

      // require - stops test on failure
      require.NoError(t, user.Save())
      assert.NotZero(t, user.ID)
  }
  ```

- **Table-Driven Tests**
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

- **Test Helpers** with `t.Helper()`
  ```go
  // Good - helper function with t.Helper()
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

- **Error Testing**
  ```go
  func TestValidate(t *testing.T) {
      // Test success path
      user := &User{Name: "Alice", Email: "alice@example.com"}
      assert.NoError(t, user.Validate())

      // Test error paths
      user = &User{Name: "", Email: "alice@example.com"}
      err := user.Validate()
      assert.Error(t, err)
      assert.Contains(t, err.Error(), "name")

      // Check wrapped errors
      if errors.Is(err, ErrInvalidName) {
          t.Log("Got expected error type")
      }
  }
  ```

- **Subtests for isolation**
  ```go
  func TestCache(t *testing.T) {
      cache := NewCache()

      t.Run("Set and Get", func(t *testing.T) {
          cache.Set("key", "value")
          got, ok := cache.Get("key")
          assert.True(t, ok)
          assert.Equal(t, "value", got)
      })

      t.Run("Get missing key", func(t *testing.T) {
          _, ok := cache.Get("missing")
          assert.False(t, ok)
      })
  }
  ```

Use `skill-creator` for creation.

### Phase 6: Go Reviewer Agent

**Deliverable**: `plugins/go-style-guide/agents/go-reviewer/AGENT.md`

Create code review agent that:

- Reviews Go code against style principles (clarity, simplicity, concision, etc.)
- Checks idiom compliance:
  - Error handling patterns (`if err := ...; err != nil`)
  - Naming conventions (MixedCaps, package names, no "Get" prefix)
  - Formatting (gofmt compliance)
  - Context usage
  - Interface acceptance vs concrete return

- Validates testing patterns:
  - Table-driven tests where appropriate
  - testify usage assertions
  - Test naming conventions
  - Test helper usage with `t.Helper()`

- Enforces user's preferences:
  - golangci-lint v2 guidance
  - Testify usage for assertions

- Reviews for:
  - Incorrect error strings (capitalization/punctuation)
  - Missing error checks
  - Improper naming (underscores in identifiers)
  - Getter methods with "Get" prefix
  - Panic usage for normal errors
  - Context passed as non-first parameter
  - Interface as return type

### Phase 7: Marketplace Registration

**Deliverable**: Update `.claude-plugin/marketplace.json`

```json
{
  "name": "zoumo-dotclaude",
  "owner": {
    "name": "zoumo",
    "email": "jim.zoumo@gmail.com"
  },
  "metadata": {
    "description": "Personal Claude Code plugins marketplace",
    "pluginRoot": "./plugins"
  },
  "repository": "https://github.com/zoumo/dotclaude",
  "plugins": [
    {
      "name": "chinese-style-guide",
      "source": "./chinese-style-guide",
      "description": "Chinese style guide for consistent formatting and spacing",
      "version": "1.0.0",
      "author": {
        "name": "zoumo",
        "email": "jim.zoumo@gmail.com"
      },
      "category": "style-guide",
      "keywords": ["chinese", "style", "formatting", "spacing"]
    },
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
  ]
}
```

### Phase 8: Plugin README

**Deliverable**: `plugins/go-style-guide/README.md`

Include:
- Installation via Claude Marketplace:
  ```bash
  /plugin marketplace add zoumo/dotclaude
  /plugin install go-style-guide
  ```

- Skill invocation:
  - `/golang-principle` - Apply Go coding principles when writing/refactoring/reviewing
  - `/golang-testing` - Apply Go testing best practices

- Agent usage for code review

- Quick reference to key rules

- Links to Google Style Guide source and Effective Go

## Acceptance Criteria

### Phase 1: Language-Agnostic Rules
- [x] `rules/common/coding-principles.md` created with enhanced principles (7 sections including DRY/KISS)
- [ ] `~/.claude/rules/common/` directory created **(requires user action - outside repo)**
- [x] Content is language-agnostic (no Go-specific code)

### Phase 2-3: Plugin Structure
- [x] Plugin directory structure created
- [x] `plugin.json` metadata created

### Phase 4: Go Principles Skill
- [x] `golang-principle/SKILL.md` created
- [x] YAML frontmatter present: name, description
- [x] Quick reference table with Good/Avoid examples (12+ examples)
- [x] Comprehensive coverage: naming, formatting, error handling, API design, code organization, common idioms

### Phase 5: Go Testing Skill
- [x] `golang-testing/SKILL.md` created
- [x] YAML frontmatter present: name, description
- [x] Quick reference table with Good/Avoid examples
- [x] Covers: table-driven tests, testify, test helpers, test naming, error testing

### Phase 6: Go Reviewer Agent
- [x] `plugins/go-style-guide/agents/go-reviewer/AGENT.md` created
- [x] Reviews against style principles
- [x] Checks idiom compliance
- [x] Validates testing patterns
- [x] Notes user's Go preferences (golangci-lint, testify)
- [x] Specific checks for common violations (Get prefix, panic usage, etc.)

### Phase 7: Registration
- [x] `.claude-plugin/marketplace.json` updated
- [x] `go-style-guide` entry added to plugins array

### Phase 8: Documentation
- [x] `README.md` created in plugin directory
- [x] Installation instructions included
- [x] Usage examples provided
- [x] Links to sources included

## Technical Considerations

- **User's Git Workflow**: Commit messages follow `feat:`, `fix:`, `docs:`, etc.
- **User's Go Preferences** (from CLAUDE.md):
  - Use golangci-lint v2
  - Use testify for tests
  - Use table-driven tests
- **Plugin Installation**: Via Claude Marketplace (`/plugin marketplace add zoumo/dotclaude`)
- **Skill Creation**: Use `skill-creator` for proper SKILL.md structure

## Code Review Findings (Post-implementation)

Review was conducted using the compound-engineering:workflows:review workflow with multiple specialized agents.

### Resolved Findings

**P2-1: Missing Security Guidance** - FIXED
- Added "Security-Aware Error Messages" section to `golang-principle/SKILL.md` after "Don't Panic for Normal Errors"
- Clarifies distinction between internal error messages (for logging) and user-facing errors (generic)

**P2-2: SQL Injection Risk** - FIXED
- Updated SQL example to use constant query with parameterization instead of variable query
- Removed user ID from error logging to prevent potential information disclosure

**P2-3: Missing Security Best Practices** - WAIVED (User request)
- User specified "fix all expect 6" (skip finding #3 about adding comprehensive security guidance)
- Different from P2-1 which addressed error message security

**P3-4: Duplicate Content** - FIXED
- Removed "Common Mistakes" table from `golang-principle/SKILL.md`
- Removed "Common Mistakes" table from `golang-testing/SKILL.md`
- Content duplicated the Quick Reference tables already present at the top

**P3-5: Common Violations Section** - FIXED
- Removed "Common Violations" section from `agents/go-reviewer/AGENT.md`
- Content was redundant with the review checklist already present

**P3-6: Generic Package Names** - FIXED
- Removed requirement to follow Kubernetes Go style guide
- Per user: "通用 golnag 规范代码不需要遵循 kubernetes 代码规范"

## References

- Google Go Style Guide: https://github.com/google/styleguide/tree/main/go
- Google Go Style Guide Website: https://google.github.io/styleguide/go/
- Effective Go: https://go.dev/doc/effective_go
- Go Tutorial - Testing: https://go.dev/doc/tutorial/add-a-test
- Google Python Style Guide: https://google.github.io/styleguide/pyguide
- Existing Plugin: `plugins/chinese-style-guide` (reference for structure)
- Marketplace format: `.claude-plugin/marketplace.json`

## Next Steps

**Status: COMPLETED** - All implementation phases finished and code review findings addressed.

**Code Review Summary:**
- 5 of 6 findings fixed (P2-1, P2-2, P3-4, P3-5, P3-6)
- 1 finding waived per user request (P2-3: comprehensive security guidance)

**Original Steps (for reference):**
1. Create `docs/coding-principles.md` - COMPLETED
2. Create `golang-principle` SKILL.md - COMPLETED
3. Create `golang-testing` SKILL.md - COMPLETED
4. Create `.claude-plugin/plugin.json` - COMPLETED
5. Create `go-reviewer` agent - COMPLETED
6. Update marketplace.json - COMPLETED
7. Create README.md - COMPLETED

**User Action Required:**
- Create `~/.claude/rules/common/` directory locally (outside repo sandbox)
- Copy `docs/coding-principles.md` to `~/.claude/rules/common/` if desired for global rules