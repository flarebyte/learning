## `log` + `log/slog` Cheatsheet (Go)

> 🪵 For writing logs — simple or structured — to stdout, stderr, files, etc.

---

**`log` (Standard Logging – Unstructured)**

```go
import "log"
```

### Basic Logging

```go
log.Print("Hello, log!")
log.Println("Line with newline")
log.Printf("Hello %s", "world")
```

### Logging Levels (manual)

```go
log.Println("[INFO] App started")
log.Println("[WARN] Disk space low")
log.Println("[ERROR] Something broke")
```

> 🔕 No built-in levels – use prefixes manually or wrap it

---

### Log to File

```go
f, _ := os.Create("app.log")
log.SetOutput(f)
```

### Add Prefix / Timestamp

```go
log.SetPrefix("[MyApp] ")
log.SetFlags(log.LstdFlags | log.Lshortfile)
```

- `log.LstdFlags`: date + time
- `log.Lshortfile`: filename:line
- `log.LUTC`, `log.Lmicroseconds`, etc.

---

## `log/slog` (Structured Logging – Go 1.21+)

```go
import "log/slog"
```

### Basic Structured Logs

```go
slog.Info("User logged in", "user", "alice", "role", "admin")
slog.Warn("Disk space low", "free", "100MB")
```

> 👇 Key-value pairs (JSON-ready)

---

### JSON Output (Production Mode)

```go
handler := slog.NewJSONHandler(os.Stdout, nil)
logger := slog.New(handler)
slog.SetDefault(logger)

slog.Info("Order placed", "orderID", 1234)
```

```json
{ "time": "...", "level": "INFO", "msg": "Order placed", "orderID": 1234 }
```

---

### Custom Attributes

```go
logger.Info("event", slog.String("user", "bob"), slog.Int("age", 32))
```

### Add Context with `With`

```go
userLogger := slog.Default().With("user", "bob")
userLogger.Info("Logged in")
userLogger.Warn("Invalid password")
```

---

### Handler Options

```go
opts := &slog.HandlerOptions{Level: slog.LevelDebug}
jsonHandler := slog.NewJSONHandler(os.Stdout, opts)
slog.SetDefault(slog.New(jsonHandler))
```

### 🛑 Levels

```go
slog.Debug("debugging")
slog.Info("info message")
slog.Warn("warning")
slog.Error("error occurred")
```

> ❗ No `Fatal` or `Panic` built-in — handle manually if needed

---

## Testing Log Output

```go
var buf bytes.Buffer
logger := slog.New(slog.NewTextHandler(&buf, nil))
logger.Info("Testing", "ok", true)
fmt.Println(buf.String())
```

---

## Which to Use?

| Package    | Best For                           |
| ---------- | ---------------------------------- |
| `log`      | Simple apps, scripts, legacy       |
| `log/slog` | Modern apps, structured logs, APIs |
