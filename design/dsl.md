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

## Formal Grammars (for computational purposes)

- Context-Free Grammars (CFGs):
  - These are very common, especially in computer science.
  - They use rules of the form "A → α," where A is a non-terminal symbol and α is a string of terminals and/or non-terminals.
  - CFGs are used to describe the syntax of programming languages and many aspects of natural languages.
  - A common notation for CFG's are Backus-Naur Form(BNF) and extended BNF(EBNF).
- Regular Grammars:
  - These are simpler than CFGs.
  - They can be used to describe regular languages, which are recognized by finite automata.
  - They are often used for tasks like lexical analysis (breaking a text into tokens).
- Dependency Grammars:-
  - Instead of phrase structure, dependency grammars focus on the relationships between words in a sentence.
  - They represent grammatical structure as a set of binary relations (dependencies) between words.
  - These grammars are very useful for natural language processing tasks.

## Core Components of a CFG

When describing the syntax of Context-Free Grammars (CFGs), it's important to understand the core components and how they're typically represented. Here's a breakdown:

- Terminal Symbols (Σ):
  - These are the basic symbols of the language, the "alphabet" from which strings are formed.
  - Think of them as the actual characters or tokens that appear in the final strings of the language.
  - Often represented by lowercase letters (a, b, c), digits (0, 1, 2), or special symbols (+, -, \*, /).
- Non-terminal Symbols (V):
  - These are variables or placeholders that represent syntactic categories.
  - They are used to define the structure of the language.
  - Often represented by uppercase letters (A, B, S) or descriptive names (<expression>, <sentence>).
- Production Rules (P):
  - These rules define how non-terminal symbols can be replaced by other symbols (terminal or non-terminal).
  - They have the form: A → α, where:
    - A is a non-terminal symbol.
    - α is a string of terminal and/or non-terminal symbols.
  - The "→" symbol means "can be replaced by."
- Start Symbol (S):
  - This is a special non-terminal symbol that represents the root of the grammar.
  - The generation of strings in the language starts with the start symbol.

### Typical Syntax and Notation

- Formal Definition:
  - A CFG is formally defined as a 4-tuple: G = (V, Σ, P, S).
- Production Rule Notation:
  - The most fundamental syntax is the production rule: A → α.
  - When a non-terminal has multiple possible replacements, they are often combined using the "or" symbol (|). For example: A → α | β, which means "A can be replaced by α or β."
- Backus-Naur Form (BNF) and Extended BNF (EBNF):
  - These are commonly used notations for representing CFGs, especially in computer science.
  - BNF:
    - Uses "::=" instead of "→" to define production rules.
    - Non-terminals are often enclosed in angle brackets (< >).
  - EBNF:
    - Extends BNF with additional symbols for repetition, optional elements, and grouping.
    - This makes it more concise and easier to read.

Example for a `boolean` language:

```bnf
<expression> ::= <term> | <expression> "or" <term>
<term> ::= <factor> | <term> "and" <factor>
<factor> ::= <variable> | "not" <factor> | "(" <expression> ")"
<variable> ::= "a" | "b" | "c" | "d"  // Add more variables as needed
```

**Explanation:**

- `<expression>`:
  - Represents the overall boolean expression.
  - It can be a `<term>` or a combination of two expressions joined by the "or" operator. This defines the **lowest precedence** operator.
- `<term>`:
  - Represents a boolean term.
  - It can be a `<factor>` or a combination of two terms joined by the "and" operator. This defines the **middle precedence** operator.
- `<factor>`:
  - Represents a **single** boolean factor.
  - It can be a `<variable>`, a "not" operator followed by a factor, or an expression enclosed in parentheses. This defines the **highest precedence** operator.
- **`<variable>`:**
  - Represents a boolean variable.
  - For simplicity, I've listed "a", "b", "c", and "d" as examples.

**Operator Precedence:**

This grammar implicitly defines the operator precedence:

1.  **Parentheses `( )`:** Highest precedence (evaluated first).
2.  **`not`:** Next highest precedence.
3.  **`and`:** Middle precedence.
4.  **`or`:** Lowest precedence (evaluated last).

## Top-down parsing

Top-down parsing is a parsing strategy that constructs the parse tree from the top, starting with the start symbol, and working down to the leaves. Here's a breakdown of the common types:

- Recursive Descent Parsing:
  - This is a straightforward top-down parsing technique that uses a set of recursive procedures to process the input.
  - Each procedure corresponds to a non-terminal symbol in the grammar.
  - It can involve backtracking, meaning if a choice doesn't lead to a successful parse, the parser may need to go back and try another option.
- Predictive Parsing (LL Parsing):
  - This is a type of recursive descent parsing that does not require backtracking.
  - It uses a lookahead symbol to predict which production rule to apply.
  - LL parsers are often table-driven, using a parsing table to guide the parsing process.
  - A common type is LL(1), where "1" indicates that it uses one lookahead symbol.
- LL(k) Parsers:
  - These are a generalization of LL parsers, where "k" represents the number of lookahead symbols used. LL(1) is the most commonly used, as using more lookahead symbols increases complexity.

Key Characteristics of Top-Down Parsers:

- They construct the parse tree from the root to the leaves.
- They typically perform a leftmost derivation.
- They can have issues with left-recursive grammars.

When deciding between the common types of top-down parsers, especially recursive descent and LL parsers, several factors come into play. Here's a breakdown of the key considerations:

### Grammar Characteristics:

- Left Recursion:
  - Recursive descent parsers (in their basic form) struggle with left-recursive grammars. These grammars have rules where a non-terminal symbol can start with itself, leading to infinite recursion.
  - LL parsers also cannot directly handle left recursion. Therefore, grammars need to be transformed to eliminate left recursion before using these parsers.
- Ambiguity:
  - Both recursive descent and LL parsers can have difficulties with ambiguous grammars, where a single input string can have multiple parse trees.
  - LL(1) parsers, in particular, require unambiguous grammars.
- Grammar Complexity:
  - Simple grammars are well-suited for recursive descent parsers, especially if you're writing a parser by hand.
  - More complex grammars might necessitate the use of LL parsers, often with the aid of parser generators.
  - LL(1) parsers are restricted to grammars that fulfil the LL(1) condition.

### Parsing Requirements:

- Backtracking:
  - Basic recursive descent parsers may involve backtracking, which can significantly impact performance.
  - LL parsers are designed to be predictive, avoiding backtracking, which makes them more efficient.
- Lookahead:
  - LL parsers rely on lookahead symbols to make parsing decisions. LL(1) parsers use one lookahead symbol, while LL(k) parsers use k lookahead symbols.
  - The amount of lookahead required can influence the complexity of the parser.
- Implementation Effort:
  - Recursive descent parsers can be relatively easy to implement manually, especially for simple grammars.
  - LL parsers often involve constructing parsing tables, which can be more complex. Parser generators can automate this process.
- Performance:
  - LL parsers, particularly LL(1), are generally more efficient due to their predictive nature and lack of backtracking.
  - Recursive descent parsers can be less efficient, especially with grammars that require extensive backtracking.
