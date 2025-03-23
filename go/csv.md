## `encoding/csv` Cheatsheet (Go)

> For reading and writing **CSV (Comma-Separated Values)** files  
> ✅ Works with `[]string`, files, streams, etc.

---

**Import First**

```go
import (
    "encoding/csv"
    "os"
)
```

---

**Read CSV File**

```go
f, _ := os.Open("data.csv")
defer f.Close()

r := csv.NewReader(f)
records, _ := r.ReadAll()  // [][]string
```

Each record is a row (slice of strings):

```go
for _, row := range records {
    fmt.Println(row[0], row[1])
}
```

---

**Read Line-by-Line (Streaming)**

```go
r := csv.NewReader(f)
for {
    record, err := r.Read()
    if err == io.EOF {
        break
    }
    fmt.Println(record)
}
```

> ✅ Use this for large CSVs that can't fit in memory

---

**Write CSV File**

```go
f, _ := os.Create("output.csv")
defer f.Close()

w := csv.NewWriter(f)
w.Write([]string{"Name", "Age"})
w.WriteAll([][]string{
    {"Alice", "30"},
    {"Bob", "25"},
})
w.Flush()
```

> ⚠️ Always call `Flush()` or `WriteAll()` to ensure data is written!

---

**Customizing Reader / Writer**

```go
r.Comma = ';'     // Change delimiter
r.FieldsPerRecord = -1 // Allow variable number of fields

w.UseCRLF = true  // Use \r\n line endings (Windows-style)
```

---

**Parse CSV into Structs (Manual Mapping)**

```go
type Person struct {
    Name string
    Age  string
}

var people []Person
for _, row := range records[1:] { // Skip header
    people = append(people, Person{Name: row[0], Age: row[1]})
}
```

> No built-in struct tags like `json`, but you can map manually or use third-party libs (like `gocarina/gocsv`)

---

**Handle Errors Gracefully**

```go
record, err := r.Read()
if err != nil {
    if err == io.EOF {
        break
    }
    log.Fatal(err)
}
```

---

**Read from Strings / Memory**

```go
reader := csv.NewReader(strings.NewReader("a,b,c\n1,2,3"))
records, _ := reader.ReadAll()
```

```go
var sb strings.Builder
writer := csv.NewWriter(&sb)
writer.Write([]string{"hello", "world"})
writer.Flush()
```
