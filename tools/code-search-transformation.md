# Structural Code Search and Transformation Tools

| Tool                   | Lang Support (TS, JS, Go, Py, Dart) | 🔍 Search  | 🔁 Code-mod | 🛠️ Usage (CLI/API) | Notes                                                    |
| ---------------------- | ----------------------------------- | ---------- | ----------- | ------------------ | -------------------------------------------------------- |
| **Comby**              | ✅ ✅ ✅ ✅ ⚠️ (partial Dart)       | ✅ Yes     | ✅ Yes      | CLI, Config        | DSL-style patterns. Multi-language, simple to use.       |
| **ASTGrep**            | ✅ ✅ ✅ ✅ ⚠️                      | ✅ Yes     | ✅ Yes      | CLI, Config, API   | Tree-sitter based. Fast and very flexible.               |
| **jscodeshift**        | ✅ ✅ ❌ ❌ ❌                      | ✅ Yes     | ✅ Yes      | CLI (Node.js)      | Used at Meta. Babel-based, scriptable in JS.             |
| **ts-morph**           | ✅ ✅ ❌ ❌ ❌                      | ⚠️ Partial | ✅ Yes      | API (TypeScript)   | TS wrapper for compiler API. Great for precise mods.     |
| **recast**             | ✅ ✅ ❌ ❌ ❌                      | ✅ Yes     | ✅ Yes      | API (JS/TS)        | Code gen + AST parsing. Often combined with jscodeshift. |
| **babel**              | ✅ ✅ ❌ ❌ ❌                      | ⚠️ Partial | ✅ Yes      | CLI, API           | Plugin-based transformations. Powerful, but complex.     |
| **libCST**             | ❌ ❌ ❌ ✅ ❌                      | ✅ Yes     | ✅ Yes      | API (Python)       | Meta’s structured Python tool. Very accurate.            |
| **pyastgrep**          | ❌ ❌ ❌ ✅ ❌                      | ✅ Yes     | ❌ No       | CLI, API           | Search only. Python AST grep.                            |
| **codemod (Facebook)** | ❌ ✅ ❌ ✅ ❌                      | ⚠️ Limited | ✅ Yes      | CLI, API (Python)  | Lightweight tool for Python code changes.                |
| **go/ast**             | ❌ ❌ ✅ ❌ ❌                      | ❌ No      | ✅ Yes      | API (Go)           | Native. Requires manual AST handling.                    |
| **go-rewrite / goast** | ❌ ❌ ✅ ❌ ❌                      | ✅ Yes     | ✅ Yes      | CLI, API (Go)      | Structural rewrite for Go.                               |
| **dart_codemod**       | ❌ ❌ ❌ ❌ ✅                      | ⚠️ Partial | ✅ Yes      | CLI, API (Dart)    | Dart-specific codemod library.                           |

**Legend:**

- ✅ Yes = Fully supported
- ⚠️ Partial = Somewhat supported or requires setup
- ❌ No = Not supported
- **CLI** = Command-line tool
- **API** = Programmatic access (good for custom logic)
- **Config** = Declarative, pattern-based usage (DSL or YAML)

**Best by Language**

| Language          | Best Structural Tools                                            |
| ----------------- | ---------------------------------------------------------------- |
| **JavaScript/TS** | `jscodeshift`, `ts-morph`, `recast`, `babel`                     |
| **Go**            | `go/ast`, `Comby`, `go-rewrite`, `ASTGrep`                       |
| **Python**        | `libCST`, `pyastgrep`, `codemod`, `ASTGrep`, `Comby`             |
| **Dart**          | `dart_codemod` (package), `Comby (partial)`, `ASTGrep (partial)` |
