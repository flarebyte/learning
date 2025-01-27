# Builder patterns

## Fluent builder

The **Fluent Builder** pattern allows constructing objects using a method-chaining approach. Each builder method returns the builder itself, enabling concise and readable code. It is ideal for creating objects with multiple optional configurations while maintaining clarity.

### Key Characteristics:

- Methods return the builder (`this`), supporting chaining.
- Flexible and intuitive for simple or moderately complex objects.
- Avoids enforcing a strict construction order.

### Example:

```dart
class House {
  final int windows;
  final int doors;

  House({required this.windows, required this.doors});
}

class HouseBuilder {
  int _windows = 0;
  int _doors = 0;

  HouseBuilder setWindows(int count) {
    _windows = count;
    return this;
  }

  HouseBuilder setDoors(int count) {
    _doors = count;
    return this;
  }

  House build() => House(windows: _windows, doors: _doors);
}

// Usage
final house = HouseBuilder().setWindows(4).setDoors(2).build();
```

This approach provides a clean API for object creation while ensuring configurability and simplicity.
