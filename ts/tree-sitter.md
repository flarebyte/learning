# Tree sitter

Tree-sitter is a powerful library and incremental parsing tool for code written in multiple programming languages. It is widely used in developer tools such as text editors, IDEs, and linters for efficient and accurate syntax parsing. Below is an overview of Tree-sitter, including its features, how it works, and its benefits:

Tree-sitter is a library for generating and parsing concrete syntax trees for programming languages. It provides a fast and efficient way to parse source code into a tree-like structure that represents the syntactic elements of the code. Tree-sitter supports incremental parsing, meaning it can re-parse only the changed portions of a file when edits occur, making it highly suitable for interactive applications.

## Key Features

- Multi-language Support:  
   Tree-sitter supports many programming languages, either through its official grammars or through community-maintained grammars.

- Incremental Parsing:  
   Tree-sitter is designed to re-parse only the parts of the code that have changed, rather than the entire source file, which makes it fast and suitable for real-time code analysis.

- High Accuracy:  
   It generates precise syntax trees by allowing developers to write custom grammars for each language.

- Bindings for Multiple Languages:  
   Tree-sitter has bindings for many programming languages (e.g., C, JavaScript, Python, Rust) and can be easily integrated into various tools.

- Query System:  
   Tree-sitter provides a query language to extract information from syntax trees, allowing developers to identify and manipulate specific syntactic constructs.

- Error Recovery:  
   Even with incomplete or incorrect code, Tree-sitter produces a syntax tree that provides a best-effort representation of the code, useful for interactive code editors.

## How It Works

- Grammar Definition:  
   Each language is defined by a grammar written in a special DSL provided by Tree-sitter. This grammar defines how the parser constructs a syntax tree from the source code.

- Parser Generation:  
   Using the grammar, Tree-sitter generates a parser that can produce syntax trees for the defined language.

- Parsing:  
   The parser reads source code and produces a syntax tree that represents the hierarchical structure of the code.

- Querying:  
   The syntax tree can be queried using Tree-sitter’s query language to extract nodes, analyze code structure, or apply transformations.

- Incremental Updates:  
   When the code changes, Tree-sitter updates the syntax tree incrementally by re-parsing only the affected parts.

## Benefits

- Performance:  
   Incremental parsing ensures high performance, making Tree-sitter suitable for real-time applications like code editors and live linters.

- Modularity:  
   It supports adding and combining grammars for different languages or embedded languages (e.g., HTML with embedded JavaScript).

- Error Tolerance:  
   Its ability to parse incomplete or erroneous code ensures that tools can provide meaningful feedback even during editing.

- Extensibility:  
   Custom grammars and queries allow developers to adapt Tree-sitter to their specific use cases.

## Use Cases

- Code Highlighting:  
   Text editors and IDEs use Tree-sitter to provide accurate syntax highlighting.

- Code Navigation:  
   Tools leverage the syntax tree to enable features like go-to-definition, code folding, and function/method outlines.

- Refactoring Tools:  
   By querying the syntax tree, Tree-sitter enables automated code refactoring and linting.

- Documentation Generation:  
   Developers use it to extract comments and code structure to generate documentation.

- Language Servers:  
   Tree-sitter is used in the backends of language servers to provide features like autocompletion and error diagnostics.

## Syntax

### Core Components

- Parser: The central object that parses source code and produces a syntax tree.
- Tree: The output of the parser, representing the entire source code in a hierarchical structure.
- TreeCursor: A lightweight tool for traversing the tree.
- Node: Represents a single node in the syntax tree, with properties like type, children, and ranges.
- Query: Allows pattern matching against the syntax tree.

### Setup Example

Before using Tree-sitter, ensure you’ve installed it via npm:

```bash
npm install tree-sitter
npm install tree-sitter-[language]
```

### Using the API

#### - Parser Initialization

```typescript
import Parser from "tree-sitter";
import JavaScript from "tree-sitter-javascript";

const parser = new Parser();
parser.setLanguage(JavaScript);
```

Here, we initialize the parser and set the language grammar (e.g., JavaScript). You can replace `tree-sitter-javascript` with other language grammars.

#### - Parsing Source Code

```typescript
const code = `
function greet(name) {
  return 'Hello, ' + name;
}
`;

const tree = parser.parse(code);
console.log(tree.rootNode); // Outputs the root node of the syntax tree
```

The `parse` method produces a Tree object that represents the syntax tree of the code.

#### - Inspecting Nodes

A syntax tree consists of Nodes, each representing a syntactic structure.

```typescript
const rootNode = tree.rootNode;

console.log(rootNode.type); // 'program'
console.log(rootNode.childCount); // Number of direct children
console.log(rootNode.toString()); // String representation of the node and its children

// Accessing specific children
const functionDeclaration = rootNode.firstChild;
console.log(functionDeclaration?.type); // 'function_declaration'

// Navigating properties
const funcName = functionDeclaration?.childForFieldName("name");
console.log(funcName?.text); // 'greet'
```

#### - Querying the Syntax Tree

Tree-sitter’s Query API allows you to search for specific patterns in the syntax tree.

```typescript
import { Query } from "tree-sitter";

const query = new Query(
  JavaScript,
  `
(function_declaration
  name: (identifier) @function_name)
`
);

const captures = query.matches(tree.rootNode);
for (const match of captures) {
  match.captures.forEach((capture) => {
    console.log(capture.name); // 'function_name'
    console.log(capture.node.text); // 'greet'
  });
}
```

This example queries all function declarations and captures the function names.

#### - Incremental Parsing

Tree-sitter supports incremental parsing for efficiency. When editing code, only the modified part is re-parsed.

```typescript
const newCode = `
function greet(name, age) {
  return 'Hello, ' + name + ' (' + age + ')';
}
`;

const newTree = parser.parse(newCode, tree); // Reuse the previous tree for incremental parsing
console.log(newTree.rootNode.toString());
```

#### - TreeCursor for Traversing

A TreeCursor is a lightweight object for traversing the syntax tree.

```typescript
const cursor = tree.walk();

do {
  console.log(cursor.nodeType); // Logs the type of the current node
  console.log(cursor.nodeText); // Logs the text of the current node
} while (cursor.gotoNextSibling());
```

### Key Classes and Methods

#### Parser

- `new Parser()`: Creates a parser instance.
- `setLanguage(language)`: Sets the grammar.
- `parse(input, previousTree?)`: Parses code and optionally reuses a previous tree for incremental parsing.

#### Tree

- `rootNode`: The root node of the syntax tree.
- `edit(editDescriptor)`: Adjusts the tree for incremental parsing (e.g., after text changes).

#### Node

- `type`: The type of the node (e.g., `function_declaration`).
- `text`: The code represented by the node.
- `child(index)`: Accesses a specific child node.
- `childForFieldName(name)`: Retrieves a child node by its field name.
- `range`: Provides the start and end positions of the node.

#### Query

- `new Query(language, queryString)`: Creates a query instance.
- `matches(node)`: Finds matches for the query within a syntax tree.

#### TreeCursor

- `walk()`: Creates a cursor for traversal.
- `gotoFirstChild()`, `gotoNextSibling()`, etc.: Move through the tree.

## Node types

| Node Type                | Description                                                                   |
| ------------------------ | ----------------------------------------------------------------------------- |
| `program`                | Represents the root of the syntax tree, containing all top-level constructs.  |
| `function_declaration`   | A named function declaration with a body.                                     |
| `variable_declaration`   | Declares one or more variables using `const`, `let`, or `var`.                |
| `class_declaration`      | Represents a class definition, including its name, methods, and properties.   |
| `method_definition`      | A method defined inside a class.                                              |
| `static_method`          | A static method defined inside a class.                                       |
| `arrow_function`         | Represents an arrow function (`() => {}`).                                    |
| `if_statement`           | An `if` conditional statement.                                                |
| `for_statement`          | A `for` loop construct.                                                       |
| `while_statement`        | A `while` loop construct.                                                     |
| `do_statement`           | A `do...while` loop construct.                                                |
| `switch_statement`       | A `switch` statement with multiple cases.                                     |
| `case_statement`         | A single case inside a `switch` statement.                                    |
| `return_statement`       | A `return` statement, optionally with a return value.                         |
| `import_statement`       | Represents an `import` statement for importing modules or files.              |
| `export_statement`       | Represents an `export` statement for exporting modules or definitions.        |
| `expression_statement`   | A standalone expression executed as a statement.                              |
| `call_expression`        | A function call, including its arguments.                                     |
| `new_expression`         | An instantiation of a class using the `new` keyword.                          |
| `property_access`        | Represents property access, such as `obj.property`.                           |
| `assignment_expression`  | Represents an assignment (`=`) operation.                                     |
| `binary_expression`      | Represents a binary operation (e.g., `+`, `-`, `*`, `/`, `&&`, `              | `). |
| `unary_expression`       | Represents a unary operation (e.g., `!`, `-`).                                |
| `ternary_expression`     | Represents a ternary operator (`condition ? expr1 : expr2`).                  |
| `object_expression`      | Represents an object literal.                                                 |
| `array_expression`       | Represents an array literal.                                                  |
| `enum_declaration`       | Represents an enumeration, defining a set of named constants.                 |
| `interface_declaration`  | Represents an interface declaration.                                          |
| `type_alias_declaration` | Represents a type alias declaration in TypeScript or Dart.                    |
| `getter`                 | A getter method in a class or object.                                         |
| `setter`                 | A setter method in a class or object.                                         |
| `constructor`            | The constructor method of a class.                                            |
| `namespace_declaration`  | Represents a namespace definition (TypeScript only).                          |
| `await_expression`       | Represents an `await` operation inside an asynchronous function.              |
| `yield_expression`       | Represents a `yield` statement in a generator function.                       |
| `try_statement`          | A `try...catch` or `try...finally` block.                                     |
| `catch_clause`           | The `catch` block inside a `try` statement.                                   |
| `finally_clause`         | The `finally` block inside a `try` statement.                                 |
| `throw_statement`        | Represents a `throw` statement.                                               |
| `annotation`             | A metadata annotation (commonly found in Dart).                               |
| `default_parameter`      | A parameter with a default value in a function or method.                     |
| `type_parameter`         | A generic type parameter in TypeScript or Dart.                               |
| `spread_element`         | A spread operator (`...`) used in arrays or objects.                          |
| `string_literal`         | A string literal (e.g., `"Hello"` or `'World'`).                              |
| `number_literal`         | A numeric literal (e.g., `42`).                                               |
| `boolean_literal`        | A boolean literal (`true` or `false`).                                        |
| `null_literal`           | The `null` literal.                                                           |
| `undefined`              | The `undefined` value in JavaScript/TypeScript.                               |
| `field_declaration`      | Represents a field in a Dart class.                                           |
| `import_prefix`          | A Dart-specific prefix used in imports (e.g., `import 'dart:math' as math;`). |

### - `function_declaration`

- Description: Represents a function declaration in the source code.
- Example Code:

```typescript
const code = `
function greet(name: string): string {
  return 'Hello, ' + name;
}
`;
```

### - `variable_declaration`

- Description: Represents a variable declaration, such as `const`, `let`, or `var`.
- Example Code:

```typescript
const code = `
const greeting: string = 'Hello';
`;
```

### - `class_declaration`

- Description: Represents a class declaration, including its name and body.
- Example Code:

```typescript
const code = `
class Greeter {
  greet(name: string): string {
    return 'Hello, ' + name;
  }
}
`;
```

### - `call_expression`

- Description: Represents a function call.
- Example Code:

```typescript
const code = `
greet('Alice');
`;
```

### - `if_statement`

- Description: Represents an `if` conditional statement.
- Example Code:

```typescript
const code = `
if (name === 'Alice') {
  console.log('Hello, Alice');
}
`;
```

### - `for_statement`

- Description: Represents a `for` loop.
- Example Code:

```typescript
const code = `
for (let i = - i < -; i++) {
  console.log(i);
}
`;
```

### - `return_statement`

- Description: Represents a `return` statement.
- Example Code:

```typescript
const code = `
function greet(name: string): string {
  return 'Hello, ' + name;
}
`;
```

### - `arrow_function`

- Description: Represents an arrow function.
- Example Code:

```typescript
const code = `
const greet = (name: string): string => {
  return 'Hello, ' + name;
};
`;
```

### - `expression_statement`

- Description: Represents an expression that is executed as a statement.
- Example Code:

```typescript
const code = `
console.log('Hello, world');
`;
```

### -. `import_statement`

- Description: Represents an `import` statement.
- Example Code:

```typescript
const code = `
import { greet } from './utils';
`;
```

Here's an explanation and TypeScript examples for node types representing interface declarations, static methods, and enums. These are common in languages like TypeScript, and Tree-sitter's grammar for TypeScript or JavaScript typically provides support for them.

### `interface_declaration`

- Description: Represents an interface declaration, which defines a structure for objects in TypeScript.
- Example Code:

```typescript
const code = `
interface Person {
  name: string;
  age: number;
  greet(): void;
}
`;
```

### - `static_method`

- Description: Represents a static method in a class, which is called on the class itself rather than an instance.
- Example Code:

```typescript
const code = `
class Utils {
  static add(a: number, b: number): number {
    return a + b;
  }
}
`;

const classNode = tree.rootNode.firstChild!;
console.log(classNode.type); // 'class_declaration'

const staticMethodNode = classNode
  .descendantsOfType("method_definition")
  .find((node) => node.text.startsWith("static"));
console.log(staticMethodNode?.type); // 'method_definition'
console.log(staticMethodNode?.text); // 'static add(a: number, b: number): number { return a + b; }'

const methodName = staticMethodNode?.childForFieldName("name");
console.log(methodName?.text); // 'add'
```

### - `enum_declaration`

- Description: Represents an enumeration declaration, which defines a set of named constants.
- Example Code:

```typescript
const code = `
enum Colors {
  Red = 'RED',
  Green = 'GREEN',
  Blue = 'BLUE'
}
`;

const enumNode = tree.rootNode.firstChild!;
console.log(enumNode.type); // 'enum_declaration'

const enumName = enumNode.childForFieldName("name");
console.log(enumName?.text); // 'Colors'

const enumMembers = enumNode.childForFieldName("body")?.children || [];
enumMembers.forEach((member) => {
  console.log(member.type); // 'enum_member'
  console.log(member.text); // 'Red = "RED"', 'Green = "GREEN"', 'Blue = "BLUE"'
});
```
