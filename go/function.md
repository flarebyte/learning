# Functions in Go – The Basics

Functions in Go are first-class citizens. You can define them, assign them to variables, return them, etc.

### Regular Function:

```go
func add(a int, b int) int {
    return a + b
}
```

- Parameters are **typed after the name**
- You can return multiple values (which leads into error handling)

### Multiple Return Values:

```go
func divide(a, b int) (int, error) {
    if b == 0 {
        return 0, fmt.Errorf("cannot divide by zero")
    }
    return a / b, nil
}
```

---

## Methods in Go – Functions with a Receiver

### Methods are just functions with a receiver:

```go
type User struct {
    Name string
}

func (u User) Greet() string {
    return "Hello, " + u.Name
}
```

- The receiver `(u User)` is like `self` in Python or `this` in Java.
- You can use either **value receivers** or **pointer receivers**:
  - Value receiver → copy of the struct (good for small types)
  - Pointer receiver → mutate state or avoid copying large structs

```go
func (u *User) Rename(newName string) {
    u.Name = newName
}
```

---

## Error Handling – No Exceptions, No Try/Catch

Go **does not** use exceptions for flow control. Instead, **errors are values**, and it’s your responsibility to check them.

### Idiomatic Pattern:

```go
result, err := divide(10, 0)
if err != nil {
    log.Println("Error:", err)
} else {
    fmt.Println("Result:", result)
}
```

This style:

- Makes error handling **explicit**
- Forces you to deal with errors early
- Keeps code flow predictable

### Creating Errors:

```go
errors.New("something went wrong")
fmt.Errorf("failed to connect: %v", err)
```

### Panic (only for truly exceptional situations):

```go
panic("unexpected error")
```

Avoid unless you're in a "this should never happen" situation (e.g., corrupt internal state).

---

## What Programming Paradigm is Go?

Go isn’t strictly OOP or FP. It’s **pragmatic**. Here's how it blends paradigms:

| Paradigm            | Go Support? | Notes                                                                |
| ------------------- | ----------- | -------------------------------------------------------------------- |
| **OOP**             | ✅ Partial  | Structs + Methods + Interfaces (but no classes or inheritance)       |
| **FP (Functional)** | ✅ Some     | Functions as first-class values, closures, pure functions encouraged |
| **Procedural**      | ✅ Strong   | Clean top-down flow, explicit error handling                         |
| **Generic**         | ✅ Go 1.18+ | Generics are supported with type parameters                          |

Go leans toward **composition over inheritance**, favors **interfaces over hierarchies**, and avoids complex abstractions.

---

## 💡 TL;DR Summary

- **Functions**: Regular, can return multiple values (often `result, error`)
- **Methods**: Functions with a receiver; used for behavior on structs
- **Error Handling**: No exceptions; handle `error` values explicitly
- **Paradigm**: Procedural at heart, with a dash of OOP and FP — designed for clarity, simplicity, and maintainability
