# 🗂️ `io/fs` Cheatsheet (Go)

> For **abstract filesystem interfaces** — useful for working with:
>
> - Native OS files (`os.DirFS`)
> - Embedded files (`embed.FS`)
> - Any custom file system (for testing, sandboxing, etc.)

**Import First**

```go
import "io/fs"
```

---

**Filesystem Interface**

```go
type FS interface {
    Open(name string) (fs.File, error)
}
```

- Core abstraction for any filesystem (real, embedded, or virtual)
- Can be implemented by:
  - `os.DirFS(".")`
  - `embed.FS`
  - Custom FS

---

**Read File Contents**

```go
file, err := fsys.Open("example.txt")
defer file.Close()

data, err := io.ReadAll(file) // standard file reading
```

---

**Read Directory Entries**

```go
entries, err := fs.ReadDir(fsys, ".")
for _, entry := range entries {
    fmt.Println(entry.Name(), entry.IsDir())
}
```

- `fs.ReadDir()` works like `os.ReadDir()` but uses any `fs.FS`

---

**Walk Directory Tree**

```go
fs.WalkDir(fsys, ".", func(path string, d fs.DirEntry, err error) error {
    fmt.Println(path)
    return nil
})
```

- Just like `filepath.WalkDir`, but works on **any** `fs.FS`
- Example: works with `embed.FS`, great for exploring embedded folders

---

**DirEntry Interface**

```go
type DirEntry interface {
    Name() string
    IsDir() bool
    Type() fs.FileMode
    Info() (fs.FileInfo, error)
}
```

- Returned by `ReadDir()` or `WalkDir`

---

**FileMode Constants (Permissions, Types)**

```go
const (
    fs.ModeDir        // d: is a directory
    fs.ModeSymlink    // L: is a symlink
    fs.ModePerm       // Permissions mask (os-level)
)
```

Check file type:

```go
if entry.Type().IsRegular() {
    fmt.Println("Regular file")
}
```

---

**FileInfo Interface**

```go
type FileInfo interface {
    Name() string
    Size() int64
    Mode() FileMode
    ModTime() time.Time
    IsDir() bool
    Sys() interface{}
}
```

Get `fs.FileInfo` from `entry.Info()` or `fs.Stat()`.

---

**Real Filesystem Adapter**

```go
fsys := os.DirFS(".")     // Wrap current directory as fs.FS
```

> You can now use `fs.ReadFile(fsys, "main.go")`, `fs.ReadDir(fsys, ".")`, etc.

---

**Embedded Files (via `embed.FS`)**

```go
import _ "embed"

//go:embed files/*
var embeddedFS embed.FS

fs.ReadFile(embeddedFS, "files/config.json")
```
