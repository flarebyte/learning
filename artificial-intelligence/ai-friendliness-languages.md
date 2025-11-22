# 🏆 **Programming Languages Ranked by LLM Friendliness**

| Rank   | Language           | Tier   | Why it's easy/hard for LLMs                                                     |
| ------ | ------------------ | ------ | ------------------------------------------------------------------------------- |
| **1**  | **Python**         | **S**  | Simple syntax, strong conventions, minimal ceremony, huge training corpus       |
| **2**  | **TypeScript**     | **S**  | Types help LLMs reason; consistent ecosystem compared to JS                     |
| **3**  | **SQL**            | **S**  | Declarative, small syntax surface, highly structured                            |
| **4**  | **JSON/TOML/YAML** | **S**  | Extremely regular, no logic, perfect for agents                                 |
| **5**  | **Gleam**          | **A+** | No null, no exceptions, predictable, small grammar                              |
| **6**  | **Dart**           | **A**  | Null safety + consistent OOP + simple grammar                                   |
| **7**  | **Go**             | **A−** | Tiny language; very predictable; but verbose error-handling confuses LLMs       |
| **8**  | **Rust**           | **B+** | Very explicit but complex semantics; compiler errors help LLMs self-correct     |
| **9**  | **Swift**          | **B+** | Clear syntax; but optional/ARC/complex generics can confuse models              |
| **10** | **C#**             | **B**  | Large syntax; many paradigms; but tooling is excellent                          |
| **11** | **Java**           | **B−** | Extremely verbose; models often mis-handle generics, inheritance complexity     |
| **12** | **JavaScript**     | **C**  | Tons of edge cases, implicit coercions, weird historical artifacts              |
| **13** | **Ruby**           | **C−** | Highly dynamic, monkey-patching, low predictability of runtime behavior         |
| **14** | **C++**            | **D**  | Huge, inconsistent, many footguns; LLMs produce incorrect or unsafe code easily |
| **15** | **Bash / Shell**   | **D**  | Implicit everything, whitespace-dependent, fragile, many inconsistent dialects  |

- **Tier S = extremely easy for agents**

- **Tier A = easy**

- **Tier B = manageable**

- **Tier C = often confusing**

- **Tier D = painful**

---

### ✔ **LLMs love languages that are:**

- declarative (SQL, HTML, JSON)
- simple to parse
- strongly typed in a predictable way (TypeScript, Dart, Go)
- consistent with modern conventions
- free of legacy quirks

### ✔ **LLMs struggle with languages that are:**

- too permissive or “magical” (JS, Ruby)
- full of historical baggage (C++, Bash, PHP)
- highly dynamic in unpredictable ways
