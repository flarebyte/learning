# Secrets

Storing secrets in a Flutter app requires careful handling depending on the **platform**, **security level**, and **access control**. For secure, persistent, and user-independent storage, each platform provides native solutions for **encrypted key-value storage**.

## Recommended Secure Storage Options

### Android & iOS

Use **secure storage APIs** that integrate with the platform's secure enclave:

| Platform | Storage Mechanism               | Backed By                        |
| -------- | ------------------------------- | -------------------------------- |
| Android  | Keystore + EncryptedSharedPrefs | Hardware-backed Keystore         |
| iOS      | Keychain                        | Secure Enclave / Data Protection |

> ⚠️ These systems **do not require explicit user permission**, but may prompt for device auth if configured (e.g., biometric access).

To use secure storage in Flutter, the standard approach is via the `flutter_secure_storage` plugin, which provides encrypted key-value storage using platform-specific secure storage mechanisms (Keychain on iOS, Keystore on Android).

**Usage Example**

```dart
final secureStorage = SecureStorageService();

await secureStorage.write(key: 'auth_token', value: 'abc123');
final token = await secureStorage.read('auth_token');
```

### Web

There’s **no secure, persistent storage** for secrets. Use `window.localStorage` or `IndexedDB` only for non-sensitive data. For actual security:

- Use ephemeral in-memory storage
- Delegate secret handling to a **backend**

### Desktop (Windows, macOS, Linux)

No standard, cross-platform secure storage.

- Windows: DPAPI
- macOS: Keychain
- Linux: GNOME Keyring / KWallet (if available)
  Requires platform-specific handling.

### When to Avoid Storing Secrets Locally

- **API keys**, **client secrets**, or any credentials used to **authenticate the app itself** should **never be embedded** in the client app. They can be extracted easily via reverse engineering.
- Instead, store such secrets **on a backend** and expose only controlled endpoints.
