# Flutter Platform Support Overview

Below is a concise table outlining Flutter support across major platforms in terms of **Operating Systems**, **Hardware**, **Platform Channels**, and **Feature Support** (e.g., camera, sensors):

| **Platform** | **OS Support**                       | **Hardware**     | **Platform Channels** | **Feature Support (e.g., Camera, Sensors)**                          |
| ------------ | ------------------------------------ | ---------------- | --------------------- | -------------------------------------------------------------------- |
| **Android**  | Android 5.0+ (API 21+)               | ARM, ARM64, x86  | Supported             | Full support: camera, GPS, sensors, biometrics, storage, etc.        |
| **iOS**      | iOS 11.0+                            | ARM64            | Supported             | Full support: camera, GPS, Face ID, storage, sensors                 |
| **Windows**  | Windows 10+ (x64)                    | x64 only         | Supported             | Partial: camera, microphone (via plugin), sensors limited            |
| **macOS**    | macOS 11+ (Big Sur)                  | x64, ARM64 (M1+) | Supported             | Partial: camera, microphone (via plugin), sensors limited            |
| **Linux**    | Ubuntu/Debian-based, others via Snap | x64              | Supported             | Partial: device access varies, camera possible with custom plugins   |
| **Web**      | Chrome, Safari, Edge, Firefox        | Any              | Not applicable        | Limited: No direct access to device features (camera via JS interop) |
| **Fuchsia**  | Experimental                         | ARM64            | Supported             | Experimental/unknown                                                 |

- **Platform Channels**: All native platforms (Android, iOS, Windows, macOS, Linux) support method channels for native communication.
- **Camera/Hardware Access**:
  - Android and iOS have mature support.
  - Desktop platforms rely on plugins or native integration, with varying completeness.
  - Web has no direct hardware access; limited to what's exposed via JavaScript APIs (e.g., `navigator.mediaDevices`).

## Platform-Specific Compilation (Flutter)

Use `Platform.isIOS`, `Platform.isAndroid`, etc., from `dart:io` to run platform-specific logic:

```dart
import 'dart:io';

if (Platform.isAndroid) {
  // Android
} else if (Platform.isIOS) {
  // iOS
} else if (Platform.isMacOS) {
  // macOS
} else if (Platform.isWindows) {
  // Windows
} else if (Platform.isLinux) {
  // Linux
}
```

> ⚠️ Cannot be used in const expressions or outside runtime.

**Web Detection**

```dart
import 'package:flutter/foundation.dart';

if (kIsWeb) {
  // Running on the web
}
```

- `Platform` is **not supported** on the web. Always guard its usage with `if (!kIsWeb)` to avoid runtime exceptions.

### Compile-Time Conditional Imports

Use **`dart.library.*`** environment constants:

```dart
// main.dart
import 'implementation_stub.dart'
  if (dart.library.io) 'implementation_io.dart'
  if (dart.library.html) 'implementation_web.dart';

void main() {
  doSomething();
}
```

```dart
// implementation_io.dart
void doSomething() {
  print('Running on native platform');
}
```

```dart
// implementation_web.dart
void doSomething() {
  print('Running on web');
}
```

```dart
// implementation_stub.dart
void doSomething() {
  throw UnsupportedError('Unknown platform');
}
```

### Build-Time Environment Variables

Use `--dart-define` with `const String.fromEnvironment()`:

```dart
const bool isInternalBuild = bool.fromEnvironment('INTERNAL_BUILD');
```

```bash
flutter run --dart-define=INTERNAL_BUILD=true
```
