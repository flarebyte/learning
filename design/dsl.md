# DSL

## Phases

Managing a script evaluator involves multiple phases to process and execute code effectively. Here’s a high-level overview of the key phases:

### Lexical Analysis (Tokenization)

- Purpose: Converts raw script text into a sequence of tokens (words, numbers, symbols).
- Components: Tokenizer (Lexer)
- Tasks:
  - Break input into tokens (e.g., keywords, identifiers, operators, literals).
  - Handle whitespace and comments.
  - Assign token types for syntax parsing.

### Parsing (Syntax Analysis)

- Purpose: Converts tokens into a structured representation (Abstract Syntax Tree - AST).
- Components: Parser
- Tasks:
  - Validate grammar and syntax.
  - Construct an AST.
  - Handle syntax errors.

### Semantic Analysis

- Purpose: Ensures correct meaning and logical consistency.
- Components: Semantic Analyzer
- Tasks:
  - Type checking.
  - Scope and variable resolution.
  - Function and class validation.

### Intermediate Representation (IR) Generation

- Purpose: Converts AST into an optimized intermediate form.
- Components: AST Transformer / IR Generator
- Tasks:
  - Optimize structure for execution.
  - Convert to bytecode, three-address code, or another IR.

### Optimization (Optional)

- Purpose: Improve performance before execution.
- Tasks:
  - Constant folding (precompute constants).
  - Dead code elimination.
  - Loop unrolling or function inlining.

### Execution / Code Generation

- Purpose: Convert IR into executable instructions.
- Components: Interpreter or Compiler
- Tasks:
  - Direct execution (Interpreter) or compilation to machine code.
  - Function call handling.
  - Memory management (garbage collection, if applicable).

### Runtime Management

- Purpose: Handles execution flow and resource management.
- Tasks:
  - Stack and heap management.
  - Exception handling.
  - Input/Output operations.

## Lexical Analysis Strategies

There are multiple ways to implement a tokenizer (lexer), each with its own advantages:

### Manual State Machine (Handwritten Lexer)

- Uses a state machine to process input character by character.
- Typically implemented using a switch statement or function calls based on the current state.
- Efficient for simple and complex languages, but requires careful implementation.

### Regular Expressions-Based Lexer

- Uses regex patterns to match different token types.
- Easier to implement but may be slower for complex languages.
- Requires handling edge cases where multiple patterns could match.

### Table-Driven Lexer

- Uses a predefined table of transitions (DFA – Deterministic Finite Automaton).
- Efficient but may require generating the table using tools (e.g., Flex for C).

### Lexer Generators (Tools like Lex, Flex, ANTLR, etc.)

- Automates the creation of a tokenizer using rules.
- Less flexible than a handcrafted lexer but faster to develop.

## Handling Code Positioning (Line Number, Column)

Tracking position is crucial for error reporting, debugging, and logging.

- Maintain:
  - `line_number` → Starts at `, increments on encountering `\n`.
  - `column_number` → Resets to ` at a new line, increments for each character.
- Update:
  - When encountering `\n`: `line_number += column_number =
  - For normal characters: `column_number +=
- Store position in each token (useful for debugging).

## Handling Comments

Comments should be ignored by the tokenizer but might be preserved if needed for documentation.

### Types of Comments Handling

Single-line comments (`// ...`, `# ...`)

- Ignore everything from `//` or `#` to the end of the line.
- Ensure the tokenizer advances to the next line.

Multi-line comments (`/* ... */`)

- Track `/*` start and continue until encountering `*/`.
- Must update `line_number` correctly if the comment spans multiple lines.
- Handle nested comments if the language supports them.

Docstrings (Python-style `"""..."""`)

- Might be treated as comments or as string tokens depending on language rules.

## Token Types

A tokenizer typically categorizes lexemes into different types of tokens:

### Keywords

- Reserved words (`if`, `while`, `return`, `class`, `def`).
- Recognized explicitly in the tokenizer.

### Identifiers

- Variable names, function names (`foo`, `my_variable`, `main`).
- Must follow language rules (e.g., cannot start with a digit).

### Literals

- Numbers: Integers (123), Floats (3.14).
- Strings: `"hello"`, `'world'`, raw strings, multi-line strings.
- Booleans: `true`, `false`.

### Operators

- Arithmetic (`+`, `-`, `*`, `/`, `%`).
- Logical (`&&`, `||`, `!`).
- Bitwise (`&`, `|`, `^`, `~`).
- Assignment (`=`, `+=`, `-=`).

### Delimiters and Punctuation

- Parentheses `(`, `)`, `{}`, `[]`.
- Comma `,`, Semicolon `;`, Colon `:`.

### Whitespace

- Spaces, tabs, newlines (`\n`, `\t`).
- Usually skipped but must be tracked for indentation-based languages (Python).

### Comments

- Often ignored but sometimes stored for documentation or debugging.

### End-of-File (EOF)

- Signals the end of the input stream.

## Example: Tokenizing a Simple Expression

Consider the input:

```c
int x = 10 + 5;
```

The tokenizer might produce:

| Token Type    | Lexeme | Position (Line, Col) |
| ------------- | ------ | -------------------- |
| `KEYWORD`     | `int`  | `(1,1)`              |
| `IDENTIFIER`  | `x`    | `(1,5)`              |
| `OPERATOR`    | `=`    | `(1,7)`              |
| `NUMBER`      | `10`   | `(1,9)`              |
| `OPERATOR`    | `+`    | `(1,12)`             |
| `NUMBER`      | `5`    | `(1,14)`             |
| `PUNCTUATION` | `;`    | `(1,15)`             |

## Performance Considerations

- Lookahead: Some tokens require looking ahead (e.g., `==` vs. `=`, `<=` vs. `<`).
- Buffering: Reading input in chunks instead of character-by-character improves efficiency.
- Caching: Store common tokens for faster lookup.

## Source regeneration

It is possible to reconstruct the original script from a list of tokens. This process is often called source regeneration or pretty-printing. The key requirement is that the tokenizer must preserve all necessary information, including whitespace and comments.

### Use cases

Reconstructing code from tokens is not always a required feature in a traditional compiler pipeline, but it is used in several areas:

- Code Formatting & Pretty-Printing

  - Tools like ClangFormat (C++) and Black (Python) rely on tokens to reformat code.
  - The process ensures consistent indentation, spacing, and styling.

- Syntax Highlighting & Code Editors

  - Many IDEs parse code into tokens and reconstruct sections when applying changes.
  - Enables dynamic code reformatting without altering functionality.

- Minification & Compression

  - JavaScript and CSS minifiers (e.g., UglifyJS, Terser) tokenize source code, remove unnecessary whitespace/comments, and reassemble it into a compact form.

- Code Refactoring Tools

  - Rustfmt, Prettier (JS) take tokenized source, analyze it, and regenerate formatted code.

- Transpilers & Reverse Engineering
  - Tools like Babel (JS) convert modern JavaScript into older versions by reconstructing code from transformed tokens.
  - Decompilers regenerate source code from bytecode by creating high-level tokenized representations.

### How Are Comments Managed as Tokens?

Comments are not required for execution, but they are crucial for source reconstruction and documentation. There are several ways to handle them:

#### Storing Comments as Special Tokens

- Assign a token type like `COMMENT` and store the comment text.
- Example:
  ```c
  // This is a comment
  int x = 5;  /* Another comment */
  ```
  Token list:
  ```
  [ COMMENT("// This is a comment"),
    KEYWORD("int"), IDENTIFIER("x"), OPERATOR("="),
    NUMBER("5"), PUNCTUATION(";"),
    COMMENT("/* Another comment */")
  ]
  ```

#### Attaching Comments to Neighboring Tokens

- Some formatters associate comments with the closest token.
- Example:
  ```python
  a = 5  # This is a comment
  ```
  The comment is stored alongside `a = 5`.

#### Ignoring Comments (Compiler Use Case)

- Compilers discard comments since they do not impact execution.
- But when reconstructing code, missing comments would make the output incomplete.

#### Special Handling for Docstrings

- Some languages treat multi-line comments as docstrings (e.g., Python `"""docstring"""`).
- These need to be retained as part of the AST for documentation purposes.

### Challenges in Code Regeneration

- Maintaining Formatting

  - If whitespace isn't stored, regenerated code might not match the original.
  - Solutions: Store indentation and newlines as tokens.

- Placing Comments Correctly

  - Comments can appear inline, before a statement, or inside code blocks.
  - They must be preserved in the right location.

- Handling Unnecessary Parentheses & Syntax Changes
  - Some token-based formatters might add or remove redundant parentheses, affecting readability.
