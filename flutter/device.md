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
