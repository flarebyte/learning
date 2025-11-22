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

## **Prediction: AI-Native Programming Language: Core Principles**

An AI-native language will be designed around five primary goals:

1. **Machine readability and manipulation first, humans second**
2. **Strong guardrails: type-safety, sandboxing, capability control**
3. **Fully declarative task orchestration**
4. **Composable, homoiconic, and structured**
5. **Predictable semantics — no surprises, no footguns**

In short:
⚙️ **It will resemble a hybrid of SQL, TypeScript, and Lisp — with YAML-like declarative layers and Rust-like safety guarantees.**

Expect shapes like:

```lisp
(task process_user
  :input User
  :steps [
    (load-profile id)
    (validate email)
    (notify user)
  ]
  :output Result)
```

- **Declarative workflows** encoded first-class in the language
- Imperative functions attached as optional modules
- “Main” files will look like pipeline specifications
- Agents can reorder, insert, or rewrite steps safely

Think:

```yaml
workflow:
  name: order_fulfillment
  steps:
    - validate_order
    - allocate_inventory
    - charge_payment
    - generate_invoice
    - notify_customer
```

AI-native languages will include structured intent metadata:

```lisp
(intent
  :goal \"Ensure user email is valid.\"
  :constraints [\"Do not modify password logic\"]
  :agent_scope \"validation only\")
```

Capabilities will be part of the language:

Agents will be forbidden by the compiler/sandbox from performing unapproved actions.

```lisp
(capabilities
  :read users_db
  :write logs
  :call email_service)
```

LLMs struggle with side effects; humans struggle with debugging them.

```ts
effect charge_payment(Payment p) -> Result<Receipt, PaymentError>
```

Agents can regenerate or validate tests automatically.

```lisp
(fn normalize-email
  :input String
  :output Email
  :examples [
    {input: \"TEST@MAIL.COM\", output: \"test@mail.com\"}
  ])
```

LLMs are reasoning engines. A language that embraces this may permit:

```lisp
(reasoning
  \"If the user is new, send a welcome email. Otherwise, update profile.\"
)
```

```lisp
(explainer \"This step prevents double-charging customers.\")
```
