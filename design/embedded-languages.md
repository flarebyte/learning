# Embedded Scripting Languages

| Want...                        | Best Options             |
| ------------------------------ | ------------------------ |
| 🪶 **Smallest size**           | Forth, Picoc, TinyScheme |
| 🧰 **General-purpose + small** | Lua, Wren, Squirrel      |
| 🌐 **JS in small apps**        | Duktape, QuickJS         |
| 🐍 **Python feel**             | MicroPython              |
| 👾 **Game scripting**          | Lua, Wren, Squirrel      |
| 🔒 **Security/sandboxing**     | Lua, Duktape, Wren       |

| Language          | Size (Approx) | Sandboxable | Memory Safe   | OOP             | FP  | Paradigm       | Notes / Use Case                                  |
| ----------------- | ------------- | ----------- | ------------- | --------------- | --- | -------------- | ------------------------------------------------- |
| **Lua**           | ~200–300 KB   | ✅ Easy     | ✅ Yes        | ☑️ (via tables) | ☑️  | Multi-paradigm | Very embeddable, fast, simple, game dev, IoT      |
| **Wren**          | ~200 KB       | ✅ Yes      | ✅ Yes        | ✅              | ✅  | OOP, FP        | Modern, compact, clean C API, single-file VM      |
| **Squirrel**      | ~150–250 KB   | ✅ Yes      | ✅ Yes        | ✅              | ☑️  | OOP            | C++-like, used in Valve games                     |
| **Duktape (JS)**  | ~300–400 KB   | ✅ Yes      | ✅ Yes        | ✅              | ✅  | JS style       | Tiny JS engine, great for JS-friendly embedding   |
| **QuickJS**       | ~500 KB       | ✅ Yes      | ✅ Yes        | ✅              | ✅  | JS style       | More complete JS engine (ES2020), slightly larger |
| **MicroPython**   | ~300–500 KB   | ⚠️ Tricky   | ✅ Yes        | ✅              | ✅  | Multi-paradigm | Python for microcontrollers (ESP32, STM32, etc.)  |
| **Forth**         | ~20–100 KB    | ✅ Yes      | ✅ Yes        | ❌              | ❌  | Stack-based    | Ultra-tiny, low-level, great for bootloaders      |
| **TinyScheme**    | ~100–150 KB   | ✅ Yes      | ✅ Yes        | ❌              | ✅  | Lisp/Scheme    | Very small functional language                    |
| **Picoc**         | ~80–100 KB    | ✅ Yes      | ✅ Yes        | ❌              | ❌  | Procedural     | Mini C interpreter, good for teaching or config   |
| **Tcl (TinyTcl)** | ~100 KB       | ✅ Yes      | ✅ Yes        | ☑️              | ☑️  | Procedural     | Old-school, used in automation tools              |
| **MRuby**         | ~400–600 KB   | ⚠️ Partial  | ✅ Yes        | ✅              | ☑️  | OOP            | Embedded Ruby, more memory-heavy                  |
| **ChaiScript**    | ~250 KB+      | ⚠️ Limited  | ❌ Not always | ✅              | ☑️  | OOP            | Header-only C++ scripting, good for game dev      |

**Legend:**

- ✅ = Full support / Good
- ☑️ = Partial or DIY
- ⚠️ = Possible but tricky / limited
- ❌ = Not supported or practical

## Embedded VMs

| VM Name                | Language      | Description                                                              |
| ---------------------- | ------------- | ------------------------------------------------------------------------ |
| **Lua VM**             | Lua           | Stack-based. Tiny, embeddable, mature. Part of `liblua` (C library).     |
| **LuaJIT VM**          | Lua           | Like Lua, but includes a JIT compiler. Still embeddable.                 |
| **Wren VM**            | Wren          | Clean, lightweight, register-based. One `.c` and `.h` file.              |
| **Duktape VM**         | JavaScript    | Lightweight JS VM in pure C. Easy to embed.                              |
| **QuickJS VM**         | JavaScript    | Full JS support, slightly heavier. Embeddable via C.                     |
| **MicroPython VM**     | Python        | Interprets Python bytecode on microcontrollers. VM included in firmware. |
| **TinyScheme VM**      | Scheme (Lisp) | Lisp-style interpreter/VM in C.                                          |
| **Squirrel VM**        | Squirrel      | Stack-based, compact, made for games.                                    |
| **mruby VM**           | Ruby          | Embedded Ruby VM.                                                        |
| **ChaiScript Runtime** | ChaiScript    | No actual bytecode; it interprets AST directly (slower, but flexible).   |

## Stack vs Register Based VMs

| Type           | Example       | How it works                                                    |
| -------------- | ------------- | --------------------------------------------------------------- |
| Stack-based    | Lua, Squirrel | Ops push/pop from a value stack                                 |
| Register-based | Python, Wren  | Ops read/write from virtual registers (faster but more complex) |

## Common VM Instruction Types (Opcodes)

Here’s a categorized list of the **most common instruction types**, which show up in almost every VM — from Lua to Python to your own tiny stack machine:

### Stack Manipulation

| Instruction  | Purpose                         |
| ------------ | ------------------------------- |
| `PUSH value` | Push constant onto the stack    |
| `POP`        | Remove top value from the stack |
| `DUP`        | Duplicate the top value         |
| `SWAP`       | Swap top two values             |

### ➕ Arithmetic / Logic

| Instruction        | Purpose                  |
| ------------------ | ------------------------ |
| `ADD`              | Add top two values       |
| `SUB`              | Subtract                 |
| `MUL`              | Multiply                 |
| `DIV`              | Divide                   |
| `MOD`              | Modulo                   |
| `NEG`              | Negate (unary minus)     |
| `EQ` / `NEQ`       | Compare for equality     |
| `GT` / `LT`        | Greater than / Less than |
| `AND`, `OR`, `NOT` | Boolean logic            |

### Variables / Memory

| Instruction   | Purpose                      |
| ------------- | ---------------------------- |
| `LOAD var`    | Push value of variable       |
| `STORE var`   | Pop top value and store it   |
| `LOAD_GLOBAL` | Load from global environment |
| `SET_GLOBAL`  | Store to global env          |

### Control Flow

| Instruction    | Purpose                       |
| -------------- | ----------------------------- |
| `JMP addr`     | Jump to instruction address   |
| `JMP_IF_TRUE`  | Jump if top of stack is true  |
| `JMP_IF_FALSE` | Jump if top of stack is false |
| `CALL addr`    | Call function / subroutine    |
| `RET`          | Return from function          |

### Function Handling

| Instruction | Purpose                             |
| ----------- | ----------------------------------- |
| `CALL n`    | Call function with n args           |
| `RET`       | Return from function                |
| `CLOSURE`   | Create a closure (with environment) |

### Table / Object / Array Access

| Instruction | Purpose                    |
| ----------- | -------------------------- |
| `GETTABLE`  | Get value from table/array |
| `SETTABLE`  | Set value in table/array   |
| `NEWTABLE`  | Create a new table/object  |

## ASTs vs VMs in Embedded Languages

| Feature           | AST                            | VM                                  |
| ----------------- | ------------------------------ | ----------------------------------- |
| Shape             | Tree                           | Flat list of instructions           |
| Standardization   | ❌ Usually bespoke             | ✅ Often follows patterns           |
| Level             | High-level (close to source)   | Low-level (close to hardware)       |
| Language-specific | Yes                            | Not necessarily                     |
| Optimized for     | Parsing, analysis              | Execution                           |
| Reusable?         | Only between similar languages | Reusable patterns (stack, register) |
