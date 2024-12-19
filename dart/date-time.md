# Date and time

Parsing ISO 8601 dates in Dart involves converting a string representation of a date (following the ISO 8601 standard) into a `DateTime` object. Dart's `DateTime.parse` method simplifies this process.

### Standard ISO 8601 Date Format

An ISO 8601 date string typically looks like:

- `2023-12-19T15:30:00Z` (UTC time with a "Z" suffix)
- `2023-12-19T15:30:00+05:30` (Time with an offset)
- `2023-12-19T15:30:00` (Local time, no timezone specified)

### Using `DateTime.parse`

The `DateTime.parse` method accepts most standard ISO 8601 date strings and returns a `DateTime` object.

```dart
void main() {
  // Parse an ISO 8601 string in UTC format
  String isoDateUtc = "2023-12-19T15:30:00Z";
  DateTime parsedUtc = DateTime.parse(isoDateUtc);
  print(parsedUtc); // 2023-12-19 15:30:00.000Z

  // Parse an ISO 8601 string with an offset
  String isoDateWithOffset = "2023-12-19T15:30:00+05:30";
  DateTime parsedOffset = DateTime.parse(isoDateWithOffset);
  print(parsedOffset); // 2023-12-19 10:00:00.000Z (in UTC)

  // Parse an ISO 8601 string in local time
  String isoDateLocal = "2023-12-19T15:30:00";
  DateTime parsedLocal = DateTime.parse(isoDateLocal);
  print(parsedLocal); // 2023-12-19 15:30:00.000 (local time)
}
```

### Common Scenarios

1. **Handling Timezones**  
   The `DateTime` object is stored in UTC if the input string contains a timezone indicator (`Z` or offset). For local time input, no conversion to UTC occurs.

2. **Converting Timezones**  
   Use `.toLocal()` or `.toUtc()` to convert a `DateTime` between UTC and local time:

   ```dart
   print(parsedUtc.toLocal());
   print(parsedLocal.toUtc());
   ```

3. **Formatting the Parsed Date**  
   For formatting, use the `intl` package (`DateFormat`) if you need to output the date in specific formats.

### Parsing Custom Date Strings

For non-standard ISO formats, consider using the `DateFormat` class from the `intl` package.

```dart
import 'package:intl/intl.dart';

void main() {
  String customDate = "2023-12-19 15:30:00";
  DateFormat formatter = DateFormat("yyyy-MM-dd HH:mm:ss");
  DateTime parsed = formatter.parse(customDate);
  print(parsed); // 2023-12-19 15:30:00.000
}
```

## Comparison of dates

In Dart, you can compare `DateTime` objects using relational operators and methods. `DateTime` includes a variety of utilities for comparing two dates.

### Relational Operators

You can use these operators directly to compare two `DateTime` instances:

- `==` for equality
- `!=` for inequality
- `<`, `<=` for earlier dates
- `>`, `>=` for later dates

```dart
void main() {
  DateTime date1 = DateTime(2023, 12, 19);
  DateTime date2 = DateTime(2024, 1, 1);

  print(date1 == date2); // false
  print(date1 != date2); // true
  print(date1 < date2);  // true
  print(date1 > date2);  // false
  print(date1 <= date2); // true
  print(date1 >= date2); // false
}
```

### Methods for Comparison

#### `compareTo`

The `compareTo` method returns:

- `0` if the dates are equal.
- A negative number if the current date is earlier.
- A positive number if the current date is later.

```dart
void main() {
  DateTime date1 = DateTime(2023, 12, 19);
  DateTime date2 = DateTime(2024, 1, 1);

  int result = date1.compareTo(date2);
  print(result); // -1 (date1 is earlier)
}
```

#### `isBefore` and `isAfter`

These methods provide readable comparisons for earlier or later dates.

```dart
void main() {
  DateTime date1 = DateTime(2023, 12, 19);
  DateTime date2 = DateTime(2024, 1, 1);

  print(date1.isBefore(date2)); // true
  print(date1.isAfter(date2));  // false
}
```

### Checking Same Date (Ignoring Time)

If you want to check only the date (without considering the time), compare `year`, `month`, and `day`:

```dart
void main() {
  DateTime date1 = DateTime(2023, 12, 19, 10, 30);
  DateTime date2 = DateTime(2023, 12, 19, 15, 45);

  bool isSameDate = date1.year == date2.year &&
                    date1.month == date2.month &&
                    date1.day == date2.day;

  print(isSameDate); // true
}
```

### Example: Date Range Comparison

```dart
void main() {
  DateTime startDate = DateTime(2023, 12, 19);
  DateTime endDate = DateTime(2024, 1, 1);
  DateTime targetDate = DateTime(2023, 12, 25);

  bool isWithinRange = targetDate.isAfter(startDate) && targetDate.isBefore(endDate);
  print(isWithinRange); // true
}
```

### Notes

- `DateTime` comparisons respect time zones. Convert to UTC (`.toUtc()`) if the dates have different time zones.
- Use `difference` for more granular comparisons (e.g., duration between dates).

## Duration

In Dart, you can add or subtract time from a `DateTime` instance using the `add` and `subtract` methods. These methods take a `Duration` object as input.

### Adding Time

Use the `add` method to add a `Duration` to a `DateTime`:

```dart
void main() {
  DateTime initialDate = DateTime(2023, 12, 19, 10, 0);

  // Add 5 days, 3 hours, and 30 minutes
  DateTime newDate = initialDate.add(Duration(days: 5, hours: 3, minutes: 30));
  print(newDate); // 2023-12-24 13:30:00.000
}
```

### Subtracting Time

Use the `subtract` method to subtract a `Duration` from a `DateTime`:

```dart
void main() {
  DateTime initialDate = DateTime(2023, 12, 19, 10, 0);

  // Subtract 2 days and 12 hours
  DateTime newDate = initialDate.subtract(Duration(days: 2, hours: 12));
  print(newDate); // 2023-12-16 22:00:00.000
}
```

### Creating Durations

A `Duration` object can represent a range of time using various units:

- `days`
- `hours`
- `minutes`
- `seconds`
- `milliseconds`
- `microseconds`

### Example: Add and Remove Time Ranges

```dart
void main() {
  DateTime date = DateTime(2023, 12, 19, 10, 0);

  // Add 1 week
  DateTime plusOneWeek = date.add(Duration(days: 7));
  print(plusOneWeek); // 2023-12-26 10:00:00.000

  // Subtract 3 hours and 45 minutes
  DateTime minusTime = date.subtract(Duration(hours: 3, minutes: 45));
  print(minusTime); // 2023-12-19 06:15:00.000
}
```

### Notes

- The `Duration` class does not support months or years because their lengths vary.
- To add or subtract months or years, manually adjust the `DateTime` fields:

  ```dart
  void main() {
    DateTime date = DateTime(2023, 12, 19);

    // Add 1 month
    DateTime nextMonth = DateTime(date.year, date.month + 1, date.day);
    print(nextMonth); // 2024-01-19

    // Subtract 1 year
    DateTime lastYear = DateTime(date.year - 1, date.month, date.day);
    print(lastYear); // 2022-12-19
  }
  ```

- When working with time zones, ensure consistency using `.toUtc()` or `.toLocal()` before performing calculations.

To represent a `Duration` as a string, parse it, and print it, you can use custom logic since Dart does not natively provide a `Duration` string format or parser. Here's how you can do it:

---

### Representing `Duration` as a String

A common way is to represent it in `hh:mm:ss` format.

```dart
String durationToString(Duration duration) {
  String twoDigits(int n) => n.toString().padLeft(2, '0');
  String hours = twoDigits(duration.inHours);
  String minutes = twoDigits(duration.inMinutes.remainder(60));
  String seconds = twoDigits(duration.inSeconds.remainder(60));
  return '$hours:$minutes:$seconds';
}
```

---

### Parsing a String to `Duration`

You can create a function to parse a string in the `hh:mm:ss` format.

```dart
Duration stringToDuration(String durationString) {
  List<String> parts = durationString.split(':');
  if (parts.length != 3) {
    throw FormatException('Invalid duration format. Use hh:mm:ss');
  }
  int hours = int.parse(parts[0]);
  int minutes = int.parse(parts[1]);
  int seconds = int.parse(parts[2]);
  return Duration(hours: hours, minutes: minutes, seconds: seconds);
}
```

---

### Example

Combining these two functions:

```dart
void main() {
  // Convert Duration to String
  Duration duration = Duration(hours: 2, minutes: 45, seconds: 30);
  String durationStr = durationToString(duration);
  print('Duration as String: $durationStr'); // 02:45:30

  // Parse String back to Duration
  Duration parsedDuration = stringToDuration('02:45:30');
  print('Parsed Duration: ${parsedDuration.inSeconds} seconds'); // 9930 seconds
}
```

---

### Notes

1. **Input Validation**: Ensure the input string format is valid (e.g., includes only digits and colons).
2. **Custom Formats**: Modify the `durationToString` or `stringToDuration` to support formats like `mm:ss` or `hh:mm`. Adjust parsing and formatting logic accordingly.
3. **Edge Cases**: Handle durations exceeding 24 hours or negative durations if needed.
