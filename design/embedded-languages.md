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
