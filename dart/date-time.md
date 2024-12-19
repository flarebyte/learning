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
