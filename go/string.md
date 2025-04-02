Absolutely! Here's a **practical cheatsheet for string manipulation in Go**, ideal for quick reference:

# Go String Manipulation Cheatsheet

**Basic String Operations**

```go
str := "hello"
len(str)           // Length: 5
str[0]             // First byte: 104 ('h')
string(str[1])     // Convert byte to string: "e"
```

> ❗ Strings are UTF-8 byte slices. Indexing gives bytes, not runes (chars).

---

**Concatenation**

```go
s1 := "foo"
s2 := "bar"
s3 := s1 + s2      // "foobar"
```

---

**Contains, Prefix, Suffix, Index**

```go
import "strings"

strings.Contains("team", "ea")         // true
strings.HasPrefix("golang", "go")      // true
strings.HasSuffix("filename.txt", ".txt") // true
strings.Index("chocolate", "co")       // 2
```

---

**Replace & Repeat**

```go
strings.Replace("foo bar foo", "foo", "baz", -1) // "baz bar baz"
strings.Repeat("go", 3)                          // "gogogo"
```

---

**ToUpper / ToLower / Title**

```go
strings.ToUpper("goLang")     // "GOLANG"
strings.ToLower("GoLang")     // "golang"
strings.Title("go lang")      // "Go Lang"
```

---

**Trim, Split & Join**

```go
strings.Trim("  hello  ", " ")      // "hello"
strings.TrimSpace("\t hello \n")    // "hello"
strings.Split("a,b,c", ",")         // []string{"a", "b", "c"}
strings.Join([]string{"a", "b"}, "-") // "a-b"
```

---

**Substring (Slicing)**

```go
s := "hello"
sub := s[1:4]   // "ell" (indexes: start inclusive, end exclusive)
```

---

**Rune Support (UTF-8 safe)**

```go
runes := []rune("😊abc")
fmt.Println(len(runes))         // 4
fmt.Println(string(runes[0]))   // "😊"
```

> 👍 Use `[]rune` for multibyte character manipulation.

---

**Other Handy Functions**

```go
strings.Count("cheese", "e")       // 3
strings.Repeat("na", 3)            // "nanana"
strings.EqualFold("Go", "go")      // true (case-insensitive compare)
```

---

**Format & Interpolation**

```go
fmt.Sprintf("Hello, %s!", "world")   // "Hello, world!"
```
