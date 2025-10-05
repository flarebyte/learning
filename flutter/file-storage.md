# File Storage

## File Storage Types

**Best Practices**

- Prefer internal storage for app-private data.
- Use external storage only when user needs access (e.g., exporting files).
- Always handle missing permissions and denied access gracefully.
- Use temp storage for cache or transient data.
- Avoid storing sensitive data unencrypted on disk.

### Application Sandbox

Each app runs in its own isolated storage area.

- **Internal Storage**
  - Private to the app.
  - Automatically cleaned up on uninstall.
  - Access via `getApplicationDocumentsDirectory()` (persistent) and `getTemporaryDirectory()` (cache).
- **Temporary Directory**

  - Use for cache or short-lived files.
  - Can be cleared by the OS at any time.
  - Access via `getTemporaryDirectory()`.

- **App Support Directory**
  - Used for non-user-visible files that should persist.
  - Access via `getApplicationSupportDirectory()`.

> ✅ No permission required.  
> 🔒 Not accessible by other apps.

## External Storage (Shared Folders)

### Android

- Files can be stored in shared public directories (e.g., Downloads, Pictures).
- Use `getExternalStorageDirectory()` for app-specific files (sandboxed, removed on uninstall).
- Use `getExternalStoragePublicDirectory()` (via platform channel or `path_provider_android`) for public shared folders.

> ⚠️ Requires `READ_EXTERNAL_STORAGE` and/or `WRITE_EXTERNAL_STORAGE` permission (Android 10 and lower).  
> Scoped storage (Android 10+) limits access; `MANAGE_EXTERNAL_STORAGE` is needed for broad access (Android 11+), but discouraged.

### iOS

- No true concept of external storage.
- File sharing can be enabled using **UIFileSharingEnabled** and **LSSupportsOpeningDocumentsInPlace** in `Info.plist`.

> ⚠️ User must explicitly share files or grant access via document picker.

## Storage Limits

- **Internal Storage**: Limited by device, app-specific quota applies.
- **External Storage**: Depends on user space availability.
- No standard API to check available space; platform channels (e.g., `StatFs` on Android) are needed for detailed usage.

## Authorization

### Android

- `READ_EXTERNAL_STORAGE`, `WRITE_EXTERNAL_STORAGE`, `MANAGE_EXTERNAL_STORAGE`
- Use `Permission.storage` from `permission_handler` (if allowed)

### iOS

- No runtime permissions for file access within sandbox.
- Document picker or iCloud access requires user action.

## Path Utilities (`path` library)

> Use the `path` package (built-in in Flutter, no import needed for basic usage).

### Common Methods

```dart
basename('/foo/bar/baz.txt');         // → 'baz.txt'
dirname('/foo/bar/baz.txt');          // → '/foo/bar'
extension('/foo/bar/baz.txt');        // → '.txt'
join('/foo', 'bar', 'baz.txt');       // → '/foo/bar/baz.txt' (cross-platform)
normalize('/foo/../bar');             // → '/bar'
```

> ✅ Always use `join` and `normalize` to avoid platform-specific bugs.

## File & Directory APIs (`dart:io`)

> Used for reading/writing, listing, deleting, etc.

### File

```dart
final file = File('/path/to/file.txt');

// Check existence
await file.exists();

// Read
final contents = await file.readAsString();

// Write
await file.writeAsString('Hello World', mode: FileMode.write);

// Delete
await file.delete();
```

### Directory

```dart
final dir = Directory('/path/to/dir');

// Create
await dir.create(recursive: true);

// List contents
await for (final entity in dir.list()) {
  if (entity is File) {
    // Handle file
  }
}

// Delete
await dir.delete(recursive: true);
```

## Path Providers (Flutter)

> Use `path_provider` package (standard in Flutter) for safe access to platform directories.

### Common Functions

```dart
final tempDir = await getTemporaryDirectory();
final docDir = await getApplicationDocumentsDirectory();
final supportDir = await getApplicationSupportDirectory();
final extDir = await getExternalStorageDirectory(); // Android only
```

> These return `Directory` objects. Combine with `File` or `Directory` as needed.

## Asset Access

> Read files bundled with the app (e.g., config files, templates).

```dart
final data = await rootBundle.loadString('assets/sample.txt');
```

## Symbolic Links & File Stat

> Advanced, mostly for backend or power users.

```dart
final stat = await File('/path').stat();
print(stat.size);    // In bytes
print(stat.modified);
```
