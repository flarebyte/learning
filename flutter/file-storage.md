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
