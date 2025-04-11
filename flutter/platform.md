# Flutter Platform Support Overview

Below is a concise table outlining Flutter support across major platforms in terms of **Operating Systems**, **Hardware**, **Platform Channels**, and **Feature Support** (e.g., camera, sensors):

| **Platform**     | **OS Support**                       | **Hardware**     | **Platform Channels** | **Feature Support (e.g., Camera, Sensors)**                          |
| ---------------- | ------------------------------------ | ---------------- | --------------------- | -------------------------------------------------------------------- |
| **Android**      | Android 5.0+ (API 21+)               | ARM, ARM64, x86  | Supported             | Full support: camera, GPS, sensors, biometrics, storage, etc.        |
| **iOS**          | iOS 11.0+                            | ARM64            | Supported             | Full support: camera, GPS, Face ID, storage, sensors                 |
| **Windows**      | Windows 10+ (x64)                    | x64 only         | Supported             | Partial: camera, microphone (via plugin), sensors limited            |
| **macOS**        | macOS 11+ (Big Sur)                  | x64, ARM64 (M1+) | Supported             | Partial: camera, microphone (via plugin), sensors limited            |
| **Linux**        | Ubuntu/Debian-based, others via Snap | x64              | Supported             | Partial: device access varies, camera possible with custom plugins   |
| **Web**          | Chrome, Safari, Edge, Firefox        | Any              | Not applicable        | Limited: No direct access to device features (camera via JS interop) |
| **Fuchsia**      | Experimental                         | ARM64            | Supported             | Experimental/unknown                                                 |
| **Raspberry Pi** | Experimental                         | ARM64            | Supported             | CPU-bound rendering and no hardware acceleration                     |

- **Platform Channels**: All native platforms (Android, iOS, Windows, macOS, Linux) support method channels for native communication.
- **Camera/Hardware Access**:
  - Android and iOS have mature support.
  - Desktop platforms rely on plugins or native integration, with varying completeness.
  - Web has no direct hardware access; limited to what's exposed via JavaScript APIs (e.g., `navigator.mediaDevices`).
- Flutter officially supports Linux desktop, and since **Raspberry Pi OS** is based on Debian Linux, it is possible to run Flutter apps on it as desktop applications.

## Common Features That Require Authorization

| Feature            | Requires Authorization | Example Permission              |
| ------------------ | ---------------------- | ------------------------------- |
| Platform check     | ❌                     | –                               |
| Location           | ✅                     | `ACCESS_FINE_LOCATION`          |
| Camera             | ✅                     | `CAMERA`                        |
| Microphone         | ✅                     | `RECORD_AUDIO`                  |
| File system (ext.) | ✅                     | `READ_EXTERNAL_STORAGE`         |
| Contacts           | ✅                     | `READ_CONTACTS`                 |
| Bluetooth          | ✅                     | `BLUETOOTH_CONNECT`, `LOCATION` |

### Location

- Accessing GPS or location data.
- Requires runtime permissions on Android (`ACCESS_FINE_LOCATION`, `ACCESS_COARSE_LOCATION`) and privacy settings in iOS (`NSLocationWhenInUseUsageDescription`).

### Camera & Microphone

- Capturing photos, videos, or audio.
- Needs `CAMERA`, `RECORD_AUDIO` permissions on Android and `NSCameraUsageDescription`, `NSMicrophoneUsageDescription` on iOS.

### Storage / File Access

- Reading or writing to device storage.
- Requires `READ_EXTERNAL_STORAGE`, `WRITE_EXTERNAL_STORAGE` on older Android versions (before scoped storage was enforced).

### Contacts, Calendar, SMS, Phone

- Reading contacts or calendar events, sending SMS, or making phone calls.
- All require separate, sensitive permissions.

### Bluetooth & Nearby Devices

- Scanning or connecting to nearby devices.
- Requires permissions like `BLUETOOTH_SCAN`, `BLUETOOTH_CONNECT`, and location access on Android.

## Platform GPU Support Overview

| Platform                       | GPU Usage                                                                                                                                   |
| ------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------- |
| **Android/iOS**                | ✅ Full GPU acceleration via Skia/OpenGL or Vulkan                                                                                          |
| **Windows/macOS**              | ✅ GPU acceleration via Skia with platform APIs                                                                                             |
| **Linux (x64)**                | ✅ GPU acceleration via OpenGL (with proper drivers)                                                                                        |
| **Linux (ARM / Raspberry Pi)** | ⚠️ **Not by default** – Flutter on Raspberry Pi uses **software rendering** via CPU unless a custom embedder is created to enable GPU usage |

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

## Overview of `MethodChannel` in Flutter

`MethodChannel` enables communication between Dart (Flutter) and platform-specific code (Kotlin/Java on Android, Swift/Objective-C on iOS). This is Flutter’s standard mechanism for executing **native platform functionality** not exposed by the Flutter framework.

**Use Cases**

- Accessing platform-specific APIs (e.g., biometrics, encryption, Bluetooth, camera).
- Performing secure operations (e.g., key storage, native encryption).
- Implementing performance-critical features.

**Best Practices**

- Always use `try/catch` on Dart side to handle platform exceptions.
- Validate input and output types carefully.
- Use unique and well-namespaced channel names (e.g., `com.example.security/encryption`).
- Keep platform logic modular and testable.

## Basic Structure

### Dart Side

```dart
const MethodChannel _channel = MethodChannel('your_channel_name');

Future<T> callNative<T>(String method, [dynamic arguments]) async {
  return await _channel.invokeMethod<T>(method, arguments);
}
```

### Android (Kotlin)

```kotlin
class MainActivity : FlutterActivity() {
  private val CHANNEL = "your_channel_name"

  override fun configureFlutterEngine(flutterEngine: FlutterEngine) {
    super.configureFlutterEngine(flutterEngine)
    MethodChannel(flutterEngine.dartExecutor.binaryMessenger, CHANNEL).setMethodCallHandler {
      call, result ->
      when (call.method) {
        "encrypt" -> {
          val text = call.argument<String>("text") ?: ""
          val encrypted = encrypt(text)
          result.success(encrypted)
        }
        else -> result.notImplemented()
      }
    }
  }
}
```

### iOS (Swift)

```swift
class AppDelegate: FlutterAppDelegate {
  private let channelName = "your_channel_name"

  override func application(
    _ application: UIApplication,
    didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?
  ) -> Bool {
    let controller = window?.rootViewController as! FlutterViewController
    let channel = FlutterMethodChannel(name: channelName, binaryMessenger: controller.binaryMessenger)

    channel.setMethodCallHandler { (call, result) in
      switch call.method {
        case "encrypt":
          if let args = call.arguments as? [String: Any],
             let text = args["text"] as? String {
            result("encrypted-\(text)") // Replace with actual encryption
          } else {
            result(FlutterError(code: "INVALID", message: "Missing text", details: nil))
          }
        default:
          result(FlutterMethodNotImplemented)
      }
    }
    return super.application(application, didFinishLaunchingWithOptions: launchOptions)
  }
}
```

## Data Types Supported

Dart and platform sides can exchange:

- `int`, `double`, `bool`, `String`
- `List`, `Map`, and `null`

> Complex data should be converted to JSON or structured as Map<String, dynamic>.

## Api not supported

Here's a table listing APIs that are **natively supported in iOS, Android, Web, and/or Desktop**, but **not supported natively in Flutter** (i.e., they require manual integration via platform channels or third-party plugins). These are platform-specific capabilities for which Flutter does not provide a first-party abstraction or cross-platform support out of the box.

| **API Category**            | **iOS Native** | **Android Native** | **Web Native** | **Desktop Native** | **Flutter Native Support**                  |
| --------------------------- | -------------- | ------------------ | -------------- | ------------------ | ------------------------------------------- |
| Push Notifications          | ✅             | ✅                 | ✅ (limited)   | ❌                 | ❌ (plugin required)                        |
| Background Services         | ✅             | ✅                 | ❌             | ❌                 | ❌ (plugin required)                        |
| Bluetooth / BLE             | ✅             | ✅                 | ❌             | ✅ (partial)       | ❌                                          |
| NFC                         | ✅             | ✅                 | ❌             | ❌                 | ❌                                          |
| In-App Purchases            | ✅             | ✅                 | ❌             | ❌                 | ❌                                          |
| Health APIs                 | ✅ (HealthKit) | ✅ (Google Fit)    | ❌             | ❌                 | ❌                                          |
| App Lifecycle Events        | ✅             | ✅                 | ✅             | ✅                 | ❌ (partially via `WidgetsBindingObserver`) |
| Native UI Views             | ✅             | ✅                 | ✅ (DOM)       | ✅                 | ❌ (requires PlatformView)                  |
| Platform Authentication     | ✅             | ✅                 | ✅             | ✅                 | ❌                                          |
| Keychain / Keystore         | ✅             | ✅                 | ❌             | ✅                 | ❌                                          |
| Background Audio            | ✅             | ✅                 | ✅             | ✅                 | ❌                                          |
| VPN / Network Extensions    | ✅             | ✅                 | ❌             | ✅                 | ❌                                          |
| App Shortcuts / Intents     | ✅             | ✅                 | ✅ (PWA)       | ❌                 | ❌                                          |
| App Widgets / Extensions    | ✅             | ✅                 | ❌             | ❌                 | ❌                                          |
| Dynamic Links / URL Schemes | ✅             | ✅                 | ✅             | ✅                 | ❌                                          |
| File Picker (Native UI)     | ✅             | ✅                 | ✅             | ✅                 | ❌                                          |
| Device Admin APIs           | ❌             | ✅                 | ❌             | ❌                 | ❌                                          |
| Accessibility Services      | ✅             | ✅                 | ✅             | ✅                 | ❌ (partially via semantics)                |

> ✅ = Supported natively on the platform  
> ❌ = Not supported or limited  
> **Flutter Native Support** = Does Flutter provide this functionality out of the box, without plugins or platform channels?

Below is a focused table for **AI, Machine Learning, and Encryption APIs**, showing whether they are natively supported on each platform (iOS, Android, Web, Desktop), and whether Flutter supports them **natively without requiring plugins, FFI, or platform channels**.

| **API Category**               | **iOS Native**        | **Android Native**  | **Web Native**           | **Desktop Native**       | **Flutter Native Support** |
| ------------------------------ | --------------------- | ------------------- | ------------------------ | ------------------------ | -------------------------- |
| On-device ML Inference         | ✅ Core ML            | ✅ ML Kit / NNAPI   | ❌ (Web ML is limited)   | ❌ (custom integrations) | ❌                         |
| Model Downloading              | ✅                    | ✅                  | ✅                       | ✅                       | ❌                         |
| Image Classification           | ✅ Core ML            | ✅ ML Kit           | ❌                       | ❌                       | ❌                         |
| Speech Recognition             | ✅ SFSpeechRecognizer | ✅ SpeechRecognizer | ✅ Web Speech API        | ✅ (via OS APIs)         | ❌                         |
| NLP (Text analysis)            | ✅ NaturalLanguage    | ✅ ML Kit           | ✅ (some JS libs)        | ❌                       | ❌                         |
| Face Detection                 | ✅ Vision Framework   | ✅ ML Kit           | ✅ (limited in JS)       | ❌                       | ❌                         |
| Biometric Auth (Face/Touch ID) | ✅ LocalAuth          | ✅ BiometricPrompt  | ❌                       | ✅ (OS dependent)        | ❌                         |
| Asymmetric Encryption          | ✅ SecKey API         | ✅ AndroidKeyStore  | ✅ Web Crypto API        | ✅ OpenSSL, native APIs  | ❌                         |
| Symmetric Encryption           | ✅ CommonCrypto       | ✅ javax.crypto     | ✅ Web Crypto API        | ✅ (OpenSSL, etc.)       | ❌                         |
| Secure Storage                 | ✅ Keychain           | ✅ Keystore         | ❌ (storage is insecure) | ✅ Platform-specific     | ❌                         |

> ✅ = Natively supported by the platform  
> ❌ = Not supported or requires custom/platform-specific code  
> **Flutter Native Support** = Out-of-the-box Dart or Flutter support (no plugins or platform code)
