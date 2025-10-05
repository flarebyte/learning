# `shared_preferences`

### What is `shared_preferences`?

`shared_preferences` is a Flutter plugin that provides a simple way to **persist key-value pairs** of primitive data types. It’s most commonly used to **store user preferences**, like settings, flags, or session data, that you want to retain between app launches.

### Supported Data Types

You can store and retrieve:
- `int`
- `double`
- `bool`
- `String`
- `List<String>`

### Basic Usage

```dart
import 'package:shared_preferences/shared_preferences.dart';

// Saving data
void saveData() async {
  final prefs = await SharedPreferences.getInstance();
  await prefs.setBool('isLoggedIn', true);
  await prefs.setString('username', 'flutter_dev');
}

// Retrieving data
void loadData() async {
  final prefs = await SharedPreferences.getInstance();
  bool? isLoggedIn = prefs.getBool('isLoggedIn');
  String? username = prefs.getString('username');
}

// Removing data
void removeData() async {
  final prefs = await SharedPreferences.getInstance();
  await prefs.remove('isLoggedIn');
}

// Clearing all data
void clearAll() async {
  final prefs = await SharedPreferences.getInstance();
  await prefs.clear();
}
```

### Things to Keep in Mind

- It's not meant for **large data storage** or complex object storage.
- It uses **platform-specific persistent storage** (like `NSUserDefaults` on iOS/macOS and `SharedPreferences` on Android).
- Stored data is **synchronous** once it's loaded — you only `await` when getting the instance.

### Common Use Cases

- Remembering **theme preference** (dark/light).
- Storing **login state** or tokens (non-sensitive!).
- Keeping track of **onboarding completion**.
- Saving **last visited screen or settings**.
