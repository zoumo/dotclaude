---
name: golang-principle
description: Use when writing, refactoring, or reviewing Go code. Ensures idiomatic Go coding standards including naming conventions, error handling, API design, and code organization.
---

# Go Coding Principles

## Overview

Idiomatic Go coding standards based on Google's [Effective Go](https://go.dev/doc/effective_go) and [Go Style Guide](https://google.github.io/styleguide/go/). These principles promote readability, maintainability, and consistency across Go codebases.

## When to Use

Apply these guidelines when:
- Writing new Go code
- Refactoring existing Go code
- Reviewing Go code changes
- Designing Go APIs and packages

## Quick Reference

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

## Naming Conventions

### Package Names

Package names should be lowercase, single words, no underscores or mixedCaps:

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

### Exported vs Unexported

Exported names start with capital letter, unexported with lowercase:

```go
// Good
type User struct { ... }           // exported
type userCache struct { ... }       // unexported
func Process() {}                  // exported
func processCache() {}              // unexported

// Avoid
func process() for public API      // should be Process()
func Process() for private usage   // should be process()
```

### Getters

Getter methods should be named after the field they return, no "Get" prefix:

```go
// Good
func (u *User) Name() string
func (u *User) Age() int

// Avoid
func (u *User) GetName() string
func (u *User) GetAge() int
```

### Interfaces

Single-method interfaces typically end with "-er" suffix:

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

### Constants

Use MixedCaps with `iota` for enumerations:

```go
// Good
const (
    Read  = 1 << iota  // 1
    Write              // 2
    Exec               // 4
    Private            // 8
)

// Avoid
const (
    Read = 1
    Write = 2
    Exec = 4
)
```

### Receiver Names

Use short abbreviations, consistent across methods:

```go
// Good
func (b *Buffer) Read(p []byte) (n int, err error) { ... }
func (b *Buffer) Write(p []byte) (n int, err error) { ... }
func (t *Tray) Add(item string) { ... }

// Avoid
func (buffer *Buffer) Read(p []byte) (n int, err error) { ... }
func (buf *Buffer) Write(p []byte) (n int, err error) { ... }
```

## Error Handling

### Always Check Errors

Never ignore errors with `_`:

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

### Error as Last Parameter

Return error as the last parameter:

```go
// Good
func Parse(input string) (*Config, error)
func Save(ctx context.Context, data []byte) error

// Avoid
func (error, *Config) Parse(input string)
func Save(data []byte, ctx context.Context) error
```

### Error Messages

Error strings should not be capitalized (unless starting with exported name, proper noun, or acronym) and should not end with punctuation:

```go
// Good
return fmt.Errorf("invalid user ID: %d", id)
return fmt.Errorf("context canceled while waiting for response")

// Avoid
return fmt.Errorf("Invalid user ID: %d.", id)
return fmt.Errorf("Validation error: invalid input.")
```

### Error Wrapping

Use `%w` to wrap errors, preserving the error chain:

```go
// Good - constant query with parameterization
const getUserQuery = `SELECT * FROM users WHERE id = $1`
if err := db.QueryRow(getUserQuery, id).Scan(&user); err != nil {
    return fmt.Errorf("failed to query user: %w", err)
}
// Use errors.Is to check wrapped errors
if errors.Is(err, sql.ErrNoRows) {
    return ErrNotFound
}

// Avoid - using variable query (potentially vulnerable)
// query := fmt.Sprintf("SELECT * FROM users WHERE id = %d", id)
```

### Don't Panic for Normal Errors

Use error returns, not panics for expected error conditions:

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

### Security-Aware Error Messages

Distinguish between internal error messages for logging and user-facing error responses:

```go
// Good - internal error for logging/troubleshooting
// Note: Keep detailed errors internal, expose generic messages to users
logger.Error("failed to query user", "id", id, "error", err)
return fmt.Errorf("user not found")

// Good - user-facing error
return fmt.Errorf("user not found")
```

Internal errors may include details for debugging; external errors should be generic to prevent information disclosure.

## API Design

### Context as First Parameter

For methods that may block, pass `context.Context` as the first parameter:

```go
// Good
func Fetch(ctx context.Context, id int) (*User, error)
func Process(ctx context.Context, data []byte) error

// Avoid
func Fetch(id int, ctx context.Context) (*User, error)
```

### Accept Interfaces, Return Concrete

Functions should accept interfaces but return concrete types when appropriate:

```go
// Good
func Write(w io.Writer, data []byte) error
func Read(r io.Reader, buf []byte) (int, error)

// Avoid (usually)
func Write() (io.Writer, error)
```

### Package Names

Avoid generic package names like `util`, `helper`, `common`:

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

### Godoc Formatting

Exported functions should have godoc comments that start with the function name and form complete sentences:

```go
// Good - starts with function name, full sentence
// Parse parses the input string and returns a Config.
// It returns an error if the input is malformed.
func Parse(input string) (*Config, error) { ... }

// Avoid
// This function parses the input.
func Parse(input string) (*Config, error) { ... }
```

## Code Organization

### Formatting

Always run `gofmt` on all source files. The canonical formatting rules are built into gofmt:

- No fixed line length - refactor instead of splitting lines unnecessarily
- MixedCaps naming (camel case), no underscores (except tests/generated code)

### Imports Grouping

Organize imports in groups: stdlib, third-party, project:

```go
import (
    "fmt"
    "os"

    "github.com/stretchr/testify/assert"

    "myproject/internal/user"
)
```

### Function Size

Keep functions small and focused, with single responsibilities.

## Common Idioms

### short Variable Declaration

Use `:=` for assignment where possible:

```go
// Good
name, err := getName()
count := len(items)

// Avoid when variable already exists
if name, err := getName(); err != nil {
    return err  // name is shadowed, this is OK
}
```

### Error Handling Idiom

The standard error handling pattern:

```go
// Good - common idiom
if err := validate(input); err != nil {
    return err
}

if err := doSomething(); err != nil {
    return fmt.Errorf("context: %w", err)
}
```

## References

- [Effective Go](https://go.dev/doc/effective_go)
- [Google Go Style Guide](https://google.github.io/styleguide/go/)
- [Go Code Review Comments](https://github.com/golang/go/wiki/CodeReviewComments)