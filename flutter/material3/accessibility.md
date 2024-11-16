# Accessibility in Flutter

Here’s a list of Material 3 widgets in Flutter that require specific actions for accessibility:

### Icon

**Action:**

- Add a semantic label using the `semanticLabel` property to provide context for screen readers.

```dart
Icon(
  Icons.home,
  semanticLabel: 'Home',
);
```

---

### IconButton

**Action:**

- Always provide a descriptive `tooltip` for screen readers and long-press feedback.
- Use `onPressed` to avoid a disabled state unless necessary.

```dart
IconButton(
  icon: Icon(Icons.search),
  tooltip: 'Search items',
  onPressed: () => print('Search pressed'),
);
```

---

### ElevatedButton, OutlinedButton, TextButton

**Action:**

- Ensure that the `child` widget conveys the button’s purpose (e.g., `Text`).
- Avoid icons without descriptive labels or use an `Icon` with `semanticLabel`.

```dart
ElevatedButton(
  onPressed: () => print('Submit'),
  child: Text('Submit'),
);
```

---

### FloatingActionButton

**Action:**

- Provide a `tooltip` to describe the button's function.
- Avoid empty `child` or icons without meaning.

```dart
FloatingActionButton(
  onPressed: () => print('Add item'),
  tooltip: 'Add new item',
  child: Icon(Icons.add, semanticLabel: 'Add'),
);
```

---

### Image

**Action:**

- Use the `semanticsLabel` property to describe the image’s purpose or set `excludeFromSemantics: true` if decorative.

```dart
Image.asset(
  'assets/banner.png',
  semanticsLabel: 'Company banner',
);
```

---

### Slider

**Action:**

- Add a descriptive `semanticFormatterCallback` for screen readers to interpret the value meaningfully.

```dart
Slider(
  value: 5,
  min: 0,
  max: 10,
  divisions: 5,
  onChanged: (value) => print(value),
  semanticFormatterCallback: (value) => '${value.round()} stars',
);
```

---

### TextField

**Action:**

- Provide a descriptive `decoration` with `labelText`, `hintText`, or `helperText`.
- Ensure `textInputAction` matches the field’s purpose for logical keyboard navigation.

```dart
TextField(
  decoration: InputDecoration(
    labelText: 'Search',
    hintText: 'Enter search term',
  ),
);
```

---

### Checkbox, Radio, Switch

**Action:**

- Always provide a descriptive `label` using `ListTile` or `Semantics`.

```dart
CheckboxListTile(
  value: true,
  onChanged: (value) => print(value),
  title: Text('Enable notifications'),
);
```

---

### ProgressIndicator (Linear, Circular)

**Action:**

- Ensure it has a `semanticsLabel` for non-visual users, especially if indeterminate.

```dart
CircularProgressIndicator(
  semanticsLabel: 'Loading data',
);
```

---

### NavigationBar

**Action:**

- Use descriptive `label` in `NavigationDestination` for all items.

```dart
NavigationBar(
  destinations: [
    NavigationDestination(
      icon: Icon(Icons.home),
      label: 'Home',
    ),
    NavigationDestination(
      icon: Icon(Icons.settings),
      label: 'Settings',
    ),
  ],
);
```

---

### Dialog

**Action:**

- Use `Semantics` or `accessibleNavigation` flag to ensure meaningful labels.

```dart
AlertDialog(
  title: Text('Delete item'),
  content: Text('Are you sure you want to delete this item?'),
  actions: [
    TextButton(onPressed: () {}, child: Text('Cancel')),
    TextButton(onPressed: () {}, child: Text('Delete')),
  ],
);
```

## Advanced Flutter Accessibility Features Overview

#### **Semantics**

The `Semantics` widget allows you to manually annotate widgets with accessible labels, actions, and hints.

```dart
Semantics(
  label: 'Submit button',
  onTap: () => print('Submitted'),
  child: Icon(Icons.send),
);
```

#### **Custom Accessibility Actions**

You can add custom actions to extend functionality for assistive technologies.

```dart
Semantics(
  label: 'Custom button',
  customSemanticsActions: {
    CustomSemanticsAction(label: 'Long Press'): () => print('Long press'),
  },
  child: Icon(Icons.accessibility),
);
```

#### **Exclude from Semantics**

Use `ExcludeSemantics` to hide decorative or non-informative widgets from accessibility tools.

```dart
ExcludeSemantics(
  child: Icon(Icons.star),
);
```

#### **Merging Semantics**

`MergeSemantics` groups multiple widgets into a single semantic node for better readability by screen readers.

```dart
MergeSemantics(
  child: Row(
    children: [
      Icon(Icons.access_time),
      Text('2:30 PM'),
    ],
  ),
);
```

#### **Accessible Navigation**

Use `Focus` widgets to enable keyboard and accessibility focus for navigable widgets.

```dart
Focus(
  onKey: (node, event) => KeyEventResult.handled,
  child: ElevatedButton(onPressed: () {}, child: Text('Focusable')),
);
```

#### **Media Query for Accessibility**

Adapt UI to user preferences for text scaling by using `MediaQuery`.

```dart
Text(
  'Large Text Example',
  style: TextStyle(fontSize: 16 * MediaQuery.textScaleFactorOf(context)),
);
```

#### **Accessible Animations**

Respect system preferences for reduced motion using `MediaQuery`.

```dart
if (MediaQuery.of(context).reduceMotion) {
  // Provide a static alternative.
}
```

#### **TalkBack/VoiceOver Testing**

Use `flutter run --accessibility` to simulate and debug accessibility features in your app.

#### **Testing Accessibility**

Validate semantic labels, actions, and structure using the semantics tester.

```dart
testWidgets('Validate Semantics', (tester) async {
  await tester.pumpWidget(MyWidget());
  expect(find.bySemanticsLabel('Submit button'), findsOneWidget);
});
```

#### **Keyboard Navigation**

Define logical focus traversal order using `FocusTraversalGroup`.

```dart
FocusTraversalGroup(
  policy: OrderedTraversalPolicy(),
  child: Column(
    children: [
      TextField(),
      ElevatedButton(onPressed: () {}, child: Text('Submit')),
    ],
  ),
);
```

These features enable comprehensive accessibility across Flutter platforms.