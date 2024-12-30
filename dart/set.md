# Set

**Set** in Dart is an unordered collection of unique elements. It is part of the `dart:core` library and ensures that no two elements are identical. This makes it ideal for scenarios where duplicates are not allowed.

### Key Characteristics:

- **Unordered**: Elements have no specific order.
- **Unique**: Duplicate elements are automatically removed.
- **Efficient Operations**: Lookup, insertion, and removal operations are generally fast.

### Common Implementations:

1. **Set**: The default implementation, which is backed by a hash table.
2. **LinkedHashSet**: Maintains insertion order.
3. **SplayTreeSet**: Sorted based on a comparator.

### Usage:

#### Creating a Set

```dart
Set<int> numbers = {1, 2, 3}; // Using literals
Set<String> names = Set();    // Using the constructor
```

#### Adding Elements

```dart
numbers.add(4);           // Adds a single element
numbers.addAll([5, 6]);   // Adds multiple elements
```

#### Removing Elements

```dart
numbers.remove(4);        // Removes a specific element
numbers.removeAll([1, 2]); // Removes multiple elements
numbers.clear();          // Removes all elements
```

#### Accessing Elements

```dart
bool containsThree = numbers.contains(3); // Check if element exists
```

#### Iterating Over a Set

```dart
for (var number in numbers) {
  print(number);
}
```

### Common Methods

- `union(Set other)`: Combines two sets.
- `intersection(Set other)`: Finds common elements.
- `difference(Set other)`: Finds elements not in the other set.
- `length`: Returns the number of elements.

### Example

```dart
void main() {
  Set<int> a = {1, 2, 3};
  Set<int> b = {3, 4, 5};

  print(a.union(b));        // {1, 2, 3, 4, 5}
  print(a.intersection(b)); // {3}
  print(a.difference(b));   // {1, 2}
}
```
