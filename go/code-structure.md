# 📦 Go Packages & Project Structure – Practical Overview

## What is a Package?

- A **package** is just a directory with `.go` files and a `package` declaration at the top.
- Every Go file starts with:

  ```go
  package mypackage
  ```

- The **directory name is not required** to match the package name (but it's common practice).

---

### `main` Package = Entry Point

```go
package main

import "fmt"

func main() {
    fmt.Println("Hello!")
}
```

- Only code in the `main` package with a `main()` function can compile to a runnable binary.
- Everything else is a **library**.

---

### Using Packages – `import`

```go
import (
    "fmt"
    "math/rand"
    "myapp/utils"
)
```

- You import other packages by their **import path**, which is often tied to module name.
- You can rename imports:
  ```go
  import u "myapp/utils"
  u.DoSomething()
  ```

---

## Recommended Project Structure

```bash
myapp/
├── go.mod
├── main.go                # entry point (package main)
├── /cmd/
│   └── myapp/             # separate main for each CLI/app
│       └── main.go
├── /internal/             # code only usable by this module
│   └── auth/
│       └── auth.go
├── /pkg/                  # reusable libraries, public
│   └── utils/
│       └── stringutils.go
├── /api/                  # API definitions, interfaces, OpenAPI, etc.
├── /configs/              # config files or templates
├── /scripts/              # dev scripts
├── /test/                 # external or integration tests
```

> You don’t need all of these folders. This is just a scalable pattern used in production.

---

## `go mod` – Modules (Dependency Management)

### ✅ Start a new module:

```bash
go mod init github.com/yourname/myapp
```

This creates a `go.mod` file that tracks dependencies and versioning.

---

### Add dependencies:

```bash
go get github.com/some/library
```

They get saved in `go.mod` and `go.sum`.

---

## Code Organization Patterns

### Example: `utils/math.go`

```go
package utils

func Add(a, b int) int {
    return a + b
}
```

Use it like:

```go
import "myapp/utils"

result := utils.Add(2, 3)
```

---

## `internal` Package = Private to This Module

```go
myapp/internal/auth/login.go
```

You **cannot import** anything inside `internal/` from outside your module.

It enforces **module-level encapsulation**.

---

## Best Practices

| Best Practice                          | Why It Matters                      |
| -------------------------------------- | ----------------------------------- |
| Use clear package names (`auth`, `db`) | Easy to understand intent           |
| Keep packages small and focused        | Avoid giant "misc" or "utils" files |
| Don't create circular imports          | Go will not allow them              |
| Use `internal/` to hide internals      | Prevent misuse from outside modules |
| Use `cmd/` for multiple executables    | Useful for CLI tools, microservices |

---

## TL;DR Recap

| Concept     | Summary                                             |
| ----------- | --------------------------------------------------- |
| `package`   | A group of `.go` files with related code            |
| `main`      | Special package that builds a binary                |
| `import`    | Use packages by name or full path                   |
| `go.mod`    | Manages module identity and dependencies            |
| `internal/` | Private code restricted to your module              |
| `cmd/`      | Keeps entrypoints clean for multiple apps           |
| `pkg/`      | Shared, public, reusable code (e.g., for libraries) |
