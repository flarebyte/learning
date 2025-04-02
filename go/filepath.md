# `path/filepath` Cheatsheet (Go)

> ✅ For **OS-aware path operations** (unlike `path`, which is for slash-based URLs)

**Import First**

```go
import "path/filepath"
```

---

**Join Paths**

```go
filepath.Join("folder", "sub", "file.txt")
// "folder/sub/file.txt" or "folder\sub\file.txt" (OS-specific)
```

---

**Get Dir, Base, Ext, Clean**

```go
filepath.Dir("/a/b/c.txt")       // "/a/b"
filepath.Base("/a/b/c.txt")      // "c.txt"
filepath.Ext("/a/b/c.txt")       // ".txt"
filepath.Clean("/a//b/../c/")    // "/a/c"
```

---

**Absolute / Relative Paths**

```go
filepath.Abs("notes.txt")
// e.g. "/home/user/notes.txt" (returns absolute path)

filepath.Rel("/a/b", "/a/b/c/d.txt")
// "c/d.txt"
```

---

**Convert Path Separators**

```go
filepath.ToSlash("a\\b\\c.txt")  // "a/b/c.txt"
filepath.FromSlash("a/b/c.txt")  // "a\b\c.txt" (on Windows)
```

---

**Match & Glob**

```go
filepath.Match("*.txt", "notes.txt")   // true, nil
filepath.Glob("*.go")                  // []string{"main.go", "utils.go"}
```

> `Glob()` returns all files matching pattern (in current dir or relative path)

---

**Walk the File Tree**

```go
filepath.WalkDir(".", func(path string, d fs.DirEntry, err error) error {
    if !d.IsDir() {
        fmt.Println("File:", path)
    }
    return nil
})
```

> ✅ Uses `WalkDir` in Go 1.16+. Use `Walk` if using older Go versions.

---

**IsAbs Path**

```go
filepath.IsAbs("/usr/local")     // true
filepath.IsAbs("docs/report.md") // false
```

---

**Temp File/Dir Helpers (related)**

While not in `filepath`, you often combine:

```go
os.MkdirTemp("", "example")
os.CreateTemp("", "file-*.txt")
```
