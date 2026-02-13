# Go Style Guide Plugin

A comprehensive Go style guide plugin that codifies the [Google Go Style Guide](https://github.com/google/styleguide) principles, decisions, and best practices for Claude Code.

## Installation

Install via Claude Marketplace:

### 1. Add Marketplace

```bash
/plugin marketplace add zoumo/dotclaude
```

Or use the full URL:

```bash
/plugin marketplace add https://github.com/zoumo/dotclaude
```

### 2. Install Plugin

```bash
/plugin install go-style-guide@zoumo-dotclaude
```

## Skills

### golang-principle

Apply Go coding principles when writing, refactoring, or reviewing Go code.

```
/golang-principle
```

Ensures idiomatic Go coding standards including:
- Naming conventions (package names, MixedCaps, interfaces, getters)
- Error handling (error wrapping, error messages, panic avoidance)
- API design (context first, interfaces vs concrete)
- Code organization (gofmt, imports, function size)
- Common idioms (short variable declaration, error patterns)

### golang-testing

Apply Go testing best practices when writing or reviewing tests.

```
/golang-testing
```

Covers:
- Table-driven tests
- testify framework usage (assert.Equal order, assert vs require)
- Test organization (file naming, function naming)
- Test helpers (t.Helper())
- Error testing (success and error paths)

## Agent

### go-reviewer

Review Go code for compliance with style principles and idiom compliance.

Checklist covers:
- Style principles (clarity, simplicity, concision, maintainability, consistency)
- Naming conventions (no underscores, no "Get" prefix, -er interfaces)
- Error handling (error checks, wrapping, proper messages)
- Security checks (no hardcoded secrets, SQL parameterization, input validation)
- Formatting (gofmt compliance, import grouping)
- API design (context first, interface acceptance)
- Testing patterns (table-driven tests, testify usage)

## References

- [Google Go Style Guide](https://google.github.io/styleguide/go/)
- [Effective Go](https://go.dev/doc/effective_go)
- [Go Code Review Comments](https://github.com/golang/go/wiki/CodeReviewComments)

## Changelog

### v1.0.0 (2026-02-13)

- Initial release
- Added `golang-principle` skill for Go coding standards
- Added `golang-testing` skill for testing patterns
- Added `go-reviewer` agent for code review

## License

[Apache 2.0](../../LICENSE)