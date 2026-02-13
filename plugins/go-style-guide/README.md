# Go Style Guide Plugin

A comprehensive Go style guide plugin that codifies the [Google Go Style Guide](https://github.com/google/styleguide) principles, decisions, and best practices for Claude Code.

## Installation

Install via Claude Marketplace:

```bash
/plugin marketplace add zoumo/dotclaude
/plugin install go-style-guide
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
- Formatting (gofmt compliance, import grouping)
- API design (context first, interface acceptance)
- Testing patterns (table-driven tests, testify usage)

## Quick Reference

| Rule | Good | Avoid |
|------|------|-------|
| Package name | `package bytes` | `package byte_utils` |
| Getter no "Get" | `obj.Owner()` | `obj.GetOwner()` |
| Error wrapping | `fmt.Errorf("context: %w", err)` | `fmt.Errorf("context: %v", err)` |
| Test naming | `TestCalculateSum_ValidInput` | `testSum_valid` |
| Assert order | `assert.Equal(t, want, got)` | `assert.Equal(t, got, want)` |

## Key Principles

### Naming

- Package names: lowercase, single-word, no underscores
- MixedCaps for exported, mixedCaps for unexported
- Single-method interfaces: -er suffix
- Receiver names: short abbreviations, consistent

### Error Handling

- Always check errors, never ignore with `_`
- Error strings: no capitalization, no punctuation
- Use `%w` for wrapping to preserve error chains
- Don't panic for normal errors

### Testing

- Use table-driven tests for similar inputs
- Use testify/assert with `assert.Equal(t, want, got)`
- Call `t.Helper()` in test helpers
- Test both success and error paths

## References

- [Google Go Style Guide](https://google.github.io/styleguide/go/)
- [Effective Go](https://go.dev/doc/effective_go)
- [Go Code Review Comments](https://github.com/golang/go/wiki/CodeReviewComments)

## License

[MIT](../../LICENSE)