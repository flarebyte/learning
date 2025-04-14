# Structural Code Search and Transformation Tools

| Tool                                                          | Lang Support (TS, JS, Go, Py, Dart) | Structural? | Open Source? | Notes                                                                     |
| ------------------------------------------------------------- | ----------------------------------- | ----------- | ------------ | ------------------------------------------------------------------------- |
| **Comby**                                                     | ✅ ✅ ✅ ✅ ⚠️ (partial Dart)       | ✅ Yes      | ✅ Yes       | Declarative pattern matching, CLI-based. Great for many langs.            |
| **jscodeshift**                                               | ✅ ✅ ❌ ❌ ❌                      | ✅ Yes      | ✅ Yes       | Built for JavaScript/TypeScript; uses Babel.                              |
| **ts-morph**                                                  | ✅ ✅ ❌ ❌ ❌                      | ✅ Yes      | ✅ Yes       | High-level wrapper around TypeScript compiler API.                        |
| **recast**                                                    | ✅ ✅ ❌ ❌ ❌                      | ✅ Yes      | ✅ Yes       | JavaScript/TS AST parser and code generator. Often used with jscodeshift. |
| **Babel**                                                     | ✅ ✅ ❌ ❌ ❌                      | ✅ Yes      | ✅ Yes       | General-purpose JS/TS compiler with powerful plugin API.                  |
| **codemod (Facebook)**                                        | ❌ ✅ ❌ ✅ ❌                      | ⚠️ Limited  | ✅ Yes       | Python-focused, used internally at Meta.                                  |
| **pyastgrep**                                                 | ❌ ❌ ❌ ✅ ❌                      | ✅ Yes      | ✅ Yes       | Structural grep for Python ASTs.                                          |
| **libCST**                                                    | ❌ ❌ ❌ ✅ ❌                      | ✅ Yes      | ✅ Yes       | Python refactoring framework from Meta.                                   |
| **Go AST (`go/ast`)**                                         | ❌ ❌ ✅ ❌ ❌                      | ✅ Yes      | ✅ Yes       | Native Go AST manipulation.                                               |
| **Golang Rewrite (github.com/incu6us/goast) or `go-rewrite`** | ❌ ❌ ✅ ❌ ❌                      | ✅ Yes      | ✅ Yes       | Go AST transformer.                                                       |
| **Dart Codemod**                                              | ❌ ❌ ❌ ❌ ✅                      | ✅ Yes      | ✅ Yes       | Dart-specific codemod framework.                                          |
| **ASTGrep**                                                   | ✅ ✅ ✅ ✅ ⚠️                      | ✅ Yes      | ✅ Yes       | Powerful AST search tool with tree-sitter support.                        |

**Best by Language**

| Language          | Best Structural Tools                                            |
| ----------------- | ---------------------------------------------------------------- |
| **JavaScript/TS** | `jscodeshift`, `ts-morph`, `recast`, `babel`                     |
| **Go**            | `go/ast`, `Comby`, `go-rewrite`, `ASTGrep`                       |
| **Python**        | `libCST`, `pyastgrep`, `codemod`, `ASTGrep`, `Comby`             |
| **Dart**          | `dart_codemod` (package), `Comby (partial)`, `ASTGrep (partial)` |
