### Zig String Manipulation Cheat Sheet for JavaScript/TypeScript Developers

This cheat sheet is aimed at developers with experience in JavaScript or TypeScript who are new to Zig. It covers basic string manipulation in Zig, explaining key concepts, syntax, and best practices. Zig's approach to strings differs from JS/TS, particularly in its use of pointers and memory management, so this guide will clarify those differences.

#### **Key Concepts**

- **Slices**: In Zig, strings are usually represented as slices (`[]const u8` for read-only strings or `[]u8` for mutable strings). A slice is essentially a view into an array, consisting of a pointer and a length. This is similar to JS arrays, but more explicit in its handling.

- **Pointers**: Zig uses pointers (`*T` for a pointer to type `T`) to reference memory locations. This can be compared to JS references, but pointers are more manual and require careful management to avoid errors like null pointer dereferences or memory leaks.

- **Memory Management**: Unlike JS, Zig does not have a garbage collector. You manage memory manually, especially for dynamic strings. For many operations, it's essential to use Zig's standard library allocators correctly.

#### **Basic String Operations**

1. **String Concatenation**

   ```zig
   const std = @import("std");

   pub fn main() void {
       var arena = std.heap.ArenaAllocator.init(std.heap.page_allocator);
       defer arena.deinit();

       const allocator = arena.allocator();
       const part1 = "Hello, ";
       const part2 = "World!";
       const combined = try std.mem.concat(allocator, part1, part2);
       defer allocator.free(combined);

       std.debug.print("Combined: {s}\n", .{combined});
   }
   ```

   **Explanation**: In Zig, you often concatenate strings using the `std.mem.concat` function. You must provide an allocator for dynamic memory allocation, and remember to free the memory afterward.

   **Best Practice**: Use `const` for immutable strings and `var` for mutable ones. For small, constant strings, use `[]const u8`. For dynamically created strings, manage memory with allocators.

2. **String Interpolation**

   ```zig
   const std = @import("std");

   pub fn main() void {
       const name = "Zig";
       const version = "0.11.0";
       std.debug.print("Welcome to {s} v{s}!\n", .{name, version});
   }
   ```

   **Explanation**: Zig doesn't have string interpolation like JS's template literals. Instead, use `std.debug.print` with format specifiers like `{s}` for strings.

   **Best Practice**: Use `std.debug.print` for simple interpolations. For more complex cases, consider creating a function that builds the string using a buffer and an allocator.

3. **Check If String Starts With**

   ```zig
   const std = @import("std");

   pub fn main() void {
       const str = "ZigLang";
       const prefix = "Zig";
       if (std.mem.startsWith(u8, str, prefix)) {
           std.debug.print("String starts with {s}\n", .{prefix});
       }
   }
   ```

   **Explanation**: Use `std.mem.startsWith` to check if a string begins with a given prefix. This is similar to `startsWith` in JS.

   **Best Practice**: Use `std.mem.startsWith` for clarity and simplicity when working with slices.

4. **String Contains Another String**

   ```zig
   const std = @import("std");

   pub fn main() void {
       const str = "Hello, Zig!";
       const substr = "Zig";
       if (std.mem.contains(u8, str, substr)) {
           std.debug.print("String contains {s}\n", .{substr});
       }
   }
   ```

   **Explanation**: Use `std.mem.contains` to check if a string contains another string.

   **Best Practice**: For better performance, ensure that both strings are slices and avoid unnecessary conversions.

5. **Split String by Character**

```zig
const std = @import("std");

pub fn main() void {
    const str = "Zig,Lang,Programming";
    var it = std.mem.splitIterator(u8, str, ',');

    while (it.next()) |part| {
        std.debug.print("Part: {s}\n", .{part});
    }
}
```

**Explanation**: `std.mem.splitIterator` is now the preferred method for splitting strings by a delimiter. It returns an iterator that you can use to loop through each part of the split string. This approach is more memory-efficient and aligns better with Zig's design philosophy.

**Best Practice**: Use `splitIterator` to avoid unnecessary allocations and to handle each split part as it is produced. This method is more idiomatic and future-proof in Zig.

#### **String Declarations and Best Practices**

- **Immutable Strings (const strings)**

  ```zig
  const greeting = "Hello, Zig!";
  ```

  **Best Practice**: Use `const` for strings that do not need to change. Zig optimizes these by storing them in the read-only section of memory.

- **Mutable Strings with Fixed Size**

  ```zig
  var buffer: [64]u8 = undefined;
  var str = buffer[0..0];
  ```

  **Best Practice**: When you know the maximum size of the string, pre-allocate a buffer. This is more memory-efficient than dynamic allocations.

- **Dynamic Strings**

  ```zig
  const std = @import("std");

  pub fn main() void {
      var arena = std.heap.ArenaAllocator.init(std.heap.page_allocator);
      defer arena.deinit();

      const allocator = arena.allocator();
      const str = try allocator.dupe(u8, "Hello, Zig!");
      defer allocator.free(str);
  }
  ```

  **Best Practice**: Use dynamic strings when the size is unknown or varies. Always manage memory manually, using allocators to allocate and free memory.

#### **Common Pitfalls and Compilation Errors**

- **Null Terminators**: Zig strings are not null-terminated like C strings. Be careful when interfacing with C functions or systems expecting null-terminated strings.

- **Out-of-Bounds Access**: Zig slices do not automatically grow. Accessing an index out of bounds will cause a compilation error or runtime panic. Always check the length before access.

- **Memory Leaks**: Failing to free dynamically allocated strings will lead to memory leaks. Always pair `allocator.alloc` or `allocator.dupe` with `allocator.free`.

#### **Recommendations**

- **Use Slices Over Pointers**: For string manipulation, prefer slices (`[]u8` or `[]const u8`) over raw pointers (`*u8`). Slices carry length information, making them safer and easier to work with.

- **Leverage Compile-Time Features**: Zig can often detect and prevent common string errors at compile time. Use `comptime` features to avoid unnecessary runtime checks.

- **Consider Performance and Memory**: Zig allows fine control over memory allocation and performance. If performance is critical, use fixed-size buffers and avoid dynamic allocations. For memory efficiency, minimize heap allocations and prefer stack-based allocations.
