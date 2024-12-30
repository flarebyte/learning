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
### Common Set Comparisons

In Dart, **sets** support a variety of comparisons to analyze relationships between two sets, such as testing equality, subset/superset relationships, and shared elements. These comparisons are particularly useful when working with collections where uniqueness and relationships matter.


#### 1. **Equality Check**
Compares two sets to determine if they contain the same elements, irrespective of order.
```dart
Set<int> a = {1, 2, 3};
Set<int> b = {3, 2, 1};

bool isEqual = a == b; // true
```

#### 2. **Subset Check**
Determines if all elements of one set are present in another.
- `isSubsetOf` checks if a set is a subset of another.

```dart
Set<int> a = {1, 2};
Set<int> b = {1, 2, 3};

bool isSubset = a.every(b.contains); // true
```

#### 3. **Superset Check**
Determines if a set contains all elements of another set.
- `isSupersetOf` checks if a set is a superset of another.

```dart
Set<int> a = {1, 2, 3};
Set<int> b = {2, 3};

bool isSuperset = b.every(a.contains); // true
```

#### 4. **Disjoint Check**
Determines if two sets have no elements in common.
- `isDisjoint` checks if the sets are disjoint.

```dart
Set<int> a = {1, 2};
Set<int> b = {3, 4};

bool isDisjoint = a.intersection(b).isEmpty; // true
```

#### 5. **Union**
Combines elements from both sets into a single set.
```dart
Set<int> a = {1, 2};
Set<int> b = {2, 3};

Set<int> union = a.union(b); // {1, 2, 3}
```

#### 6. **Intersection**
Finds common elements between two sets.
```dart
Set<int> a = {1, 2};
Set<int> b = {2, 3};

Set<int> intersection = a.intersection(b); // {2}
```

#### 7. **Difference**
Finds elements in one set that are not in the other.
```dart
Set<int> a = {1, 2, 3};
Set<int> b = {2, 3};

Set<int> difference = a.difference(b); // {1}
```

### Example: Comprehensive Comparison
```dart
void main() {
  Set<int> a = {1, 2, 3};
  Set<int> b = {2, 3};

  print(a == b);                        // false
  print(b.every(a.contains));           // true (b is a subset of a)
  print(a.every(b.contains));           // false (a is not a subset of b)
  print(a.intersection(b));             // {2, 3}
  print(a.difference(b));               // {1}
  print(a.union(b));                    // {1, 2, 3}
}
```

These comparisons provide a robust way to analyze relationships and manipulate sets effectively in Dart.
