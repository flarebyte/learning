# `flag` Cheatsheet (Go)

> For parsing command-line **flags**, **arguments**, and building simple **CLIs**

---

**Import First**

```go
import "flag"
```

---

**Define Flags**

```go
var (
    name = flag.String("name", "world", "Your name")
    age  = flag.Int("age", 30, "Your age")
    cool = flag.Bool("cool", true, "Are you cool?")
)
```

- Flags are defined with:
  - `flag.String(name, default, help)`
  - `flag.Int`, `flag.Bool`, `flag.Float64`, etc.
- Returns **pointer** to the value

---

**Parse Flags**

```go
flag.Parse()
fmt.Println("Name:", *name)
fmt.Println("Age:", *age)
fmt.Println("Cool:", *cool)
```

> ✅ Call `flag.Parse()` before using values. It parses `os.Args[1:]`.

---

**CLI Usage**

```bash
go run main.go -name=Alice -age=25 -cool=false
```

---

**Positional Arguments (after flags)**

```go
flag.Parse()
args := flag.Args()  // []string of remaining non-flag arguments
```

```bash
go run main.go -name=Joe file1.txt file2.txt
// args = ["file1.txt", "file2.txt"]
```

---

**Custom Help Message**

```go
flag.Usage = func() {
    fmt.Println("Usage: myapp [options] <files>")
    flag.PrintDefaults()
}
```

```bash
go run main.go -h
```

---

**Custom Flag Types**

```go
type listFlag []string

func (l *listFlag) String() string   { return fmt.Sprint(*l) }
func (l *listFlag) Set(value string) error {
    *l = append(*l, value)
    return nil
}

var tags listFlag
flag.Var(&tags, "tag", "Add tags (can repeat)")
```

```bash
go run main.go -tag=a -tag=b
// tags = ["a", "b"]
```

---

**Default Help Output**

```go
flag.PrintDefaults()
// Automatically shows:  -name string  Your name (default "world")
```

---

**Check If Flag Was Set**

```go
flag.Visit(func(f *flag.Flag) {
    fmt.Println("Set flag:", f.Name)
})
```
