# Go Basics Cheatsheet

**Program Structure**

```go
package main

import "fmt"

func main() {
    fmt.Println("Hello, Go!")
}
```

---

**Packages & Imports**

```go
import (
    "fmt"
    "math"
)
```

---

**Variables and Constants**

```go
var x int = 10
y := 20            // short declaration (only inside functions)
const Pi = 3.14
```

---

**Basic Types**

```go
int, int8, int16, int32, int64
uint, uintptr
float32, float64
string
bool
byte  // alias for uint8
rune  // alias for int32 (Unicode code point)
```

---

**Operators**

- Arithmetic: `+ - * / %`
- Comparison: `== != < <= > >=`
- Logical: `&& || !`

---

**Control Flow**

**If**

```go
if x > 5 {
    fmt.Println("Big")
} else {
    fmt.Println("Small")
}
```

**For (only loop in Go)**

```go
for i := 0; i < 5; i++ {
    fmt.Println(i)
}
```

**While-style**

```go
for x < 10 {
    x++
}
```

**Forever**

```go
for {
    // infinite loop
}
```

**Switch**

```go
switch x {
case 1:
    fmt.Println("One")
default:
    fmt.Println("Other")
}
```

---

**Functions**

```go
func add(a int, b int) int {
    return a + b
}

// multiple return values
func divmod(a, b int) (int, int) {
    return a / b, a % b
}
```

---

**Defer, Panic, Recover**

```go
defer fmt.Println("Done") // runs at end of function

panic("Something broke")  // like throwing
recover()                 // catch panic inside defer
```

---

**Arrays, Slices, Maps**

**Array**

```go
var a [3]int = [3]int{1, 2, 3}
```

**Slice**

```go
s := []int{1, 2, 3}
s = append(s, 4)
```

**Map**

```go
m := map[string]int{"a": 1}
m["b"] = 2
delete(m, "a")
```

---

**Structs and Methods**

```go
type Point struct {
    X, Y int
}

func (p Point) Distance() int {
    return p.X*p.X + p.Y*p.Y
}
```

---

**Interfaces**

```go
type Shape interface {
    Area() float64
}
```

No "implements" keyword – you just implement methods: "If it fits, it works."

---

**Pointers**

```go
x := 5
p := &x
*p = 10 // dereference
```

---

**Go Routines & Channels**

```go
go doSomething() // run concurrently

ch := make(chan int)
ch <- 5           // send
val := <-ch       // receive
```

---

**Testing**

```go
import "testing"

func TestAdd(t *testing.T) {
    got := add(2, 3)
    want := 5
    if got != want {
        t.Errorf("got %d, want %d", got, want)
    }
}
```

Run with: `go test`

---

**Tools to Know**

```bash
go run main.go      # run a program
go build            # compile
go test             # run tests
go fmt              # format code
go mod init <name>  # start module
go get <pkg>        # install package
```
