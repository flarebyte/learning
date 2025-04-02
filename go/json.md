# `encoding/json` Cheatsheet (Go)

> For JSON serialization (`Marshal`) and deserialization (`Unmarshal`)  
> ✅ Works with structs, maps, slices, and interfaces

---

**Import First**

```go
import "encoding/json"
```

---

**Encode Struct to JSON (`Marshal`)**

```go
type User struct {
    Name string `json:"name"`
    Age  int    `json:"age"`
}

u := User{"Alice", 30}
data, _ := json.Marshal(u)  // []byte: `{"name":"Alice","age":30}`
```

- Use `json:"fieldName"` to set JSON key names
- Use `omitempty` to skip zero values

---

**Decode JSON to Struct (`Unmarshal`)**

```go
jsonStr := `{"name":"Bob","age":25}`
var u User
json.Unmarshal([]byte(jsonStr), &u)
// u = User{Name: "Bob", Age: 25}
```

---

**Pretty Print JSON**

```go
json.MarshalIndent(u, "", "  ")
// {
//   "name": "Alice",
//   "age": 30
// }
```

---

**JSON to `map[string]interface{}`**

```go
var result map[string]interface{}
json.Unmarshal([]byte(jsonStr), &result)

fmt.Println(result["name"]) // "Bob"
```

> 🔍 Useful when structure is unknown or dynamic

---

**JSON to `interface{}` (Generic Decode)**

```go
var generic interface{}
json.Unmarshal([]byte(jsonStr), &generic)

data := generic.(map[string]interface{})
fmt.Println(data["age"]) // float64(25)
```

> ❗ Numbers default to `float64` when decoding to `interface{}`

---

**Decode Array/Slice**

```go
jsonArr := `[{"name":"A"},{"name":"B"}]`
var users []User
json.Unmarshal([]byte(jsonArr), &users)
```

---

**Handle Unknown / Extra Fields**

```go
type User struct {
    Name string `json:"name"`
}

// Will silently ignore extra fields in input JSON
```

---

**Strict Decode with `Decoder`**

```go
dec := json.NewDecoder(strings.NewReader(jsonStr))
dec.DisallowUnknownFields()
err := dec.Decode(&u)  // Fails if extra fields exist
```

---

**Custom JSON Field Handling**

```go
type Custom struct {
    Timestamp time.Time
}

func (c *Custom) UnmarshalJSON(b []byte) error {
    // custom parsing here
    return nil
}
```

> For parsing non-standard formats or preprocessing values

---

**Stream Encode/Decode (`Encoder` / `Decoder`)**

```go
enc := json.NewEncoder(os.Stdout)
enc.Encode(u)

dec := json.NewDecoder(os.Stdin)
dec.Decode(&u)
```

> ✅ Great for large files or continuous streams
