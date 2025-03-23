# Go Memory Management – The Key Ideas

Go was designed to be **fast, safe, and simple** — memory management is **automatic** (thanks to garbage collection), but you still have control when you need it.

---

## Memory is Managed, Not Manual

- **Garbage Collected (GC)**: You don’t free memory manually.
- **No malloc/free** — Go takes care of allocation and release.
- But **you should still write memory-aware code** if performance matters.

---

## Stack vs Heap

### Stack:

- Fast, local, short-lived data.
- Automatically cleaned up when function exits.

### Heap:

- Longer-lived, accessible outside the function scope.
- Cleaned by the **garbage collector**.

### Go decides _where_ to allocate using **escape analysis**.

---

### Example:

```go
func foo() *int {
    x := 42     // does this "escape" the stack?
    return &x   // yes → goes to heap
}
```

If a value **escapes** the current scope (like returning its pointer), Go allocates it on the **heap**.

---

## Performance Tips That Matter

### ✅ Prefer values over pointers when:

- The type is small (like `int`, `bool`, small structs).
- You don’t need to mutate or share it.
- Avoids unnecessary heap allocation.

---

### Reuse memory with `sync.Pool`

```go
var bufPool = sync.Pool{
    New: func() interface{} {
        return make([]byte, 1024)
    },
}

buf := bufPool.Get().([]byte)
// use it
bufPool.Put(buf)
```

Great for things like buffer reuse in high-throughput systems.

---

### Minimize allocations:

Avoid creating garbage (i.e., memory the GC has to clean up) in tight loops or hot paths.

**Instead of:**

```go
for i := 0; i < 1000; i++ {
    s := fmt.Sprintf("value %d", i) // allocates every time
}
```

**Do:**

```go
b := make([]byte, 0, 64)
for i := 0; i < 1000; i++ {
    b = b[:0]
    b = append(b, []byte(fmt.Sprintf("value %d", i))...)
}
```

---

### Use Built-in Profiling Tools

#### Memory profiling:

```bash
go test -bench . -memprofile=mem.out
go tool pprof mem.out
```

#### Runtime insights:

```go
import "runtime"

var m runtime.MemStats
runtime.ReadMemStats(&m)
fmt.Printf("Alloc = %v MiB", m.Alloc/1024/1024)
```

---

## Fast Struct Design

- Group fields to minimize padding (smaller memory footprint)
- Smaller, flat data structures = better cache locality

```go
type Bad struct {
    A bool
    B int64
}

type Better struct {
    B int64
    A bool
}
```

---

## Slices vs Arrays – Performance Considerations

### Array:

- Fixed size, value semantics.

### Slice:

- Backed by array, includes length & capacity.
- Pass **by value**, but underlying data is shared (like a reference).

```go
func mutate(s []int) {
    s[0] = 999  // changes original!
}
```

Watch for **unexpected mutations** due to shared backing arrays.

---

## Avoid Common Pitfalls

| Pitfall                         | Solution                               |
| ------------------------------- | -------------------------------------- |
| Too many small heap allocations | Reuse buffers, preallocate slices/maps |
| Memory leaks via goroutines     | Use `context` to cancel / timeout      |
| High GC pause times             | Avoid bursty allocations in hot paths  |
| Slices holding references       | Clear slices after use (`s = nil`)     |

---

## TL;DR Summary

| Concept            | Takeaway                                      |
| ------------------ | --------------------------------------------- |
| Stack vs Heap      | Go decides via escape analysis                |
| Garbage Collector  | Automatic, tune by reducing allocations       |
| Pointers           | Only use when needed — may trigger heap alloc |
| Reuse memory       | Use `sync.Pool`, reuse slices                 |
| Observe & profile  | Use `pprof`, `MemStats`, benchmarks           |
| Think about layout | Struct field order matters                    |
