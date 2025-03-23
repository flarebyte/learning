# Concurrency the Go Way

## Go’s Philosophy:

> "Do not communicate by sharing memory; share memory by communicating."

This is built around:

- **Goroutines**: lightweight threads managed by Go
- **Channels**: safe communication pipes between goroutines

---

## Goroutines\*\* – "Go do this in the background"

```go
go sayHello()
```

This runs `sayHello()` **concurrently**. Think: "thread-lite", super cheap in memory (a few KB) and managed by Go’s runtime.

```go
func sayHello() {
    fmt.Println("Hello from a goroutine")
}
```

### ✋ Important:

- Goroutines **don’t wait** for each other by default.
- You need **sync mechanisms** if you want to coordinate.

---

## Channels\*\* – Communication between Goroutines

Channels are **typed pipes** for sending and receiving values safely across goroutines.

```go
ch := make(chan int)  // unbuffered channel
```

### Send and Receive:

```go
ch <- 42       // send value
x := <-ch      // receive value
```

### Channels are blocking:

- If you try to read from an empty channel → it **waits**
- If you try to write to a full channel → it **waits**

---

## Putting it Together — Example:

```go
func worker(id int, jobs <-chan int, results chan<- int) {
    for j := range jobs {
        fmt.Printf("Worker %d processing job %d\n", id, j)
        results <- j * 2
    }
}

func main() {
    jobs := make(chan int, 5)
    results := make(chan int, 5)

    for w := 1; w <= 3; w++ {
        go worker(w, jobs, results)
    }

    for j := 1; j <= 5; j++ {
        jobs <- j
    }
    close(jobs)

    for a := 1; a <= 5; a++ {
        fmt.Println("Result:", <-results)
    }
}
```

👆 This is a **classic worker pool** example — simple, elegant, and super efficient thanks to goroutines.

---

## Buffered Channels

```go
ch := make(chan int, 3)  // can hold 3 values without blocking
```

Buffered channels help when producers and consumers aren’t perfectly synchronized.

---

## `select` Statement – Like `switch`, but for Channels

Wait on **multiple channel operations**:

```go
select {
case msg := <-ch1:
    fmt.Println("Received from ch1:", msg)
case msg := <-ch2:
    fmt.Println("Received from ch2:", msg)
default:
    fmt.Println("Nothing ready")
}
```

---

## `sync.WaitGroup` – Waiting for Goroutines to Finish

```go
var wg sync.WaitGroup

wg.Add(1)
go func() {
    defer wg.Done()
    doSomething()
}()
wg.Wait()
```

Useful when you want to wait for a bunch of goroutines to finish before exiting.

---

## `context` Package – Timeouts and Cancellation

```go
ctx, cancel := context.WithTimeout(context.Background(), time.Second*2)
defer cancel()

select {
case <-ch:
    fmt.Println("Got result")
case <-ctx.Done():
    fmt.Println("Timeout or canceled")
}
```

This is **critical in real-world apps** (e.g., web servers, APIs) to prevent leaks or long-running tasks.

---

## TL;DR Recap

| Concept          | Purpose                                            |
| ---------------- | -------------------------------------------------- |
| `go f()`         | Run `f()` concurrently (like a lightweight thread) |
| `chan`           | Safely communicate between goroutines              |
| `select`         | Wait on multiple channels                          |
| `sync.WaitGroup` | Wait for a group of goroutines to finish           |
| `context`        | Handle timeouts, cancellations gracefully          |

---

## When to Use What

- Use **goroutines** to do things in parallel (e.g., fetch multiple URLs).
- Use **channels** to coordinate or collect results.
- Use **WaitGroup** when you need to wait for multiple tasks.
- Use **context** to enforce deadlines or cancellations (API requests, background jobs).
