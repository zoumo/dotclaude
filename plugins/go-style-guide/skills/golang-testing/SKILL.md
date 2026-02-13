---
name: golang-testing
description: Use when writing or reviewing Go test files (*_test.go). Covers table-driven tests, testify usage, test helpers, and proper test organization.
---

# Go Testing Patterns

## Overview

Effective Go testing practices including table-driven tests, testify framework usage, test helpers, and proper test organization based on Google's [Go Style Guide](https://google.github.io/styleguide/go/) and [Go documentation](https://go.dev/doc/tutorial/add-a-test).

## When to Use

Apply these guidelines when:
- Writing new tests
- Reviewing test changes
- Refactoring existing tests
- Designing test strategies

## Quick Reference

| Rule | Good | Avoid |
|------|------|-------|
| Test naming | `TestCalculateSum_ValidInput` | `testSum_valid` |
| Test file | `calculator_test.go` | `calculator_tests.go` |
| Test helpers | Use `t.Helper()` | Direct `t.Errorf` in helper |
| Failure messages | `got %v, want %v` | "test failed" |
| Assert parameter order | `assert.Equal(t, want, got)` | `assert.Equal(t, got, want)` |
| Table tests | Define test cases struct | Repeating test code |

## Test Organization

### File Naming

Test files should end with `_test.go` and be in the same package as the code being tested:

```go
// Good - file name matches source file
user.go
user_test.go

// Avoid
user.go
user_tests.go
userTesting.go
```

### Function Naming

Test functions must be public, start with `Test`, and describe what's being tested:

```go
// Good
func TestCalculateSum_EmptySlice(t *testing.T) { ... }
func TestCalculateSum_SingleElement(t *testing.T) { ... }

// Avoid
func testSum(t *testing.T) { ... }      // not exported
func Test_sum(t *testing.T) { ... }      // not public
func TestCalculatesum(t *testing.T) { ... } // unclear
```

## Test Framework

### Testify for Assertions

Use `github.com/stretchr/testify` for assertions:

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

**Note**: Use `assert.Equal(t, want, got)` - the `want` value comes first.

```go
// Good
assert.Equal(t, "Alice", user.Name)  // want, got

// Avoid
assert.Equal(t, user.Name, "Alice")  // got, want
```

## Table-Driven Tests

For similar inputs, use table-driven tests:

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

## Test Helpers

### Using t.Helper()

Mark helper functions with `t.Helper()` so error messages point to the caller:

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

## Error Testing

Test both success and error paths:

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

## Subtests

Use subtests for related test cases:

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

## Test Naming Best Practices

### Descriptive Names

Test names should describe the scenario being tested:

```go
// Good
func TestCalculateSum_EmptySlice(t *testing.T) { ... }
func TestCalculateSum_SingleElement(t *testing.T) { ... }
func TestCalculateSum_NegativeNumbers(t *testing.T) { ... }

// Avoid
func TestSum(t *testing.T) { ... }           // not specific enough
func Test1(t *testing.T) { ... }              // meaningless
```

### Test Helpers Should Be in Same Package

Keep test helper functions in the same package as the tests:

```go
// Good - in same package (user_test.go)
func setupUser(t *testing.T) *User { ... }

// Avoid - in different package
// This won't have access to unexported types
```

## Error Messages

Use descriptive error messages with `got/want` format:

```go
// Good
assert.Equal(t, want, got, "CalculateSum(%v) = %v, want %v", input, got, want)

// Avoid
assert.Fail(t, "test failed")
assert.Fail(t, "unexpected result")
```

## References

- [Go Tutorial - Testing](https://go.dev/doc/tutorial/add-a-test)
- [Google Go Style Guide - Testing](https://google.github.io/styleguide/go/best-practices#testing)
- [Testify Documentation](https://github.com/stretchr/testify)
- [Table-Driven Tests](https://dave.cheney.net/2019/05/07/prefer-table-driven-tests)