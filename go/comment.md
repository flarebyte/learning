# How to Add Comments in Go

Go supports two types of comments:

### 1. **Single-line Comments**

Use `//` for comments that span a single line.

```go
// This is a single-line comment
fmt.Println("Hello, World!")
```

### 2. **Multi-line (Block) Comments**

Use `/* */` for comments that span multiple lines.

```go
/*
This is a multi-line comment.
Useful for longer explanations.
*/
fmt.Println("Hello, World!")
```

## Community Recommendations and Best Practices

### Use Comments for Documentation

- Document **packages**, **functions**, **types**, **constants**, and **variables** using comments immediately before their declarations.
- Go tools like `godoc` or `go doc` parse these comments into documentation.

```go
// Add adds two integers and returns the result.
func Add(a int, b int) int {
    return a + b
}
```

> 📌 Best practice: The comment should begin with the name of the function/type being described (e.g., `Add adds...`).

### Avoid Redundant Comments

- Comments should add value or clarify intent. Don't restate what the code obviously does.

```go
// BAD: This comment is redundant
x := 10 // Set x to 10

// GOOD: Useful context
// Set x to 10, which is the default retry count
x := 10
```

### Use Comments to Explain _Why_, Not _What_

- Let the code explain **what** is being done.
- Use comments to explain **why** something is being done in a specific way, especially for non-obvious logic or hacks.

### Go Doc Comments Are Important

- For public APIs, Go documentation comments are critical.
- Tools like [`golint`](https://github.com/golang/lint) and [`staticcheck`](https://staticcheck.io/) enforce this style.

**✅ Best Practices Summary**

| Tip                    | Description                                                       |
| ---------------------- | ----------------------------------------------------------------- |
| 🧱 Start with the name | All exported items should be commented, starting with their name. |
| 🧠 Be concise & clear  | One or two sentences is often enough.                             |
| 📚 Explain purpose     | Focus on _what_ and _why_, not _how_.                             |
| ✅ Be consistent       | Keep tone and style consistent across your package.               |

**Tools That Use These Comments**

- [`go doc`](https://pkg.go.dev/cmd/go#hdr-Show_documentation_for_package_or_symbol)
- [`godoc`](https://pkg.go.dev/golang.org/x/tools/cmd/godoc)
- [`pkg.go.dev`](https://pkg.go.dev/)
- Linters like `golint`, `staticcheck`

### Avoid Block Comments for Code Explanation

- Prefer single-line `//` comments even for longer explanations, placing them above the code block.
- Reserve `/* */` for disabling blocks of code or legal info.

## Public vs Private in Go

**Public (Exported)**

- **Starts with a capital letter**.
- Accessible from **other packages**.
- Treated as part of your library's **public API**.

```go
// This function is public (exported)
func Add(a, b int) int {
    return a + b
}
```

**Private (Unexported)**

- **Starts with a lowercase letter**.
- **Only accessible inside the same package**.
- Used for internal logic or helpers.

```go
// This function is private (unexported)
func subtract(a, b int) int {
    return a - b
}
```

**Applies to:**

| Thing     | Public Example | Private Example |
| --------- | -------------- | --------------- |
| Function  | `Println`      | `formatMessage` |
| Variable  | `MaxSize`      | `timeout`       |
| Struct    | `User`         | `config`        |
| Field     | `Name string`  | `id int`        |
| Interface | `Reader`       | `logger`        |

> 📌 **Note:** Even inside a public struct, fields must be capitalized to be exported.

```go
type User struct {
    ID   int    // Exported
    name string // Not exported (private)
}
```
