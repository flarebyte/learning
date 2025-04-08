# CBOR

### What is CBOR?

**CBOR** stands for **Concise Binary Object Representation**. It is a **binary data serialization format**, designed to be:

- **Small in size** (compact)
- **Fast to encode and decode**
- **Simple** yet able to represent complex data structures

CBOR is defined in [RFC 8949](https://datatracker.ietf.org/doc/html/rfc8949) (originally RFC 7049).

### Key Features

- **Binary format**: Unlike JSON or XML, which are text-based, CBOR is binary—more efficient for storage and transmission.
- **Self-describing**: Data items carry information about their type and structure.
- **Schema-less**: Like JSON, no predefined schema is required.
- **Supports rich data types**, including:
  - Integers (signed/unsigned)
  - Floating-point numbers
  - Strings (text and byte)
  - Arrays and maps (objects)
  - Booleans, null, undefined
  - Tagged data for extension/custom types

### CBOR vs JSON

| Feature        | CBOR             | JSON          |
| -------------- | ---------------- | ------------- |
| Format         | Binary           | Text          |
| Compactness    | More compact     | Verbose       |
| Speed          | Faster parsing   | Slower        |
| Data types     | More types       | Limited types |
| Human-readable | ❌               | ✅            |
| Custom tags    | ✅ (via tagging) | ❌            |

### Basic Example

A simple CBOR encoding of `{ "name": "Alice", "age": 30 }` would look something like this (in hex):

```
A2                # map of 2 pairs
  64             # text string of 4 chars
    6E616D65     # "name"
  65             # text string of 5 chars
    416C696365   # "Alice"
  63             # text string of 3 chars
    616765       # "age"
  18 1E          # unsigned int 30
```

### Use Cases

- IoT devices and embedded systems (due to its compactness)
- Protocols like **COSE** (CBOR Object Signing and Encryption)
- Applications needing efficient network communication
- Alternative to JSON in constrained environments (e.g., CoAP)

### Libraries and Language Support

| Language                 | Popular CBOR Library / Package          | Notes                                                 |
| ------------------------ | --------------------------------------- | ----------------------------------------------------- |
| **Python**               | `cbor2`, `cbor`, `cbor-python`          | `cbor2` is most actively maintained and feature-rich  |
| **JavaScript / Node.js** | `cbor`                                  | Supports streaming, tags, and all CBOR data types     |
| **TypeScript**           | `cbor` (same as JS)                     | Type-safe usage with types defined in DefinitelyTyped |
| **Go**                   | `fxamacker/cbor`                        | Standards-compliant, fast, and secure implementation  |
| **Java**                 | `com.fasterxml.jackson.dataformat.cbor` | Integrates with Jackson data-binding                  |
| **Dart**                 | `cbor`                                  | Available via `pub.dev`, supports encoding/decoding   |
| **C/C++**                | `libcbor`, `cn-cbor`, `tinycbor`        | `tinycbor` is popular for embedded systems            |
| **Rust**                 | `serde_cbor`                            | Built on `serde` for easy (de)serialization           |

### Overview of COSE (CBOR Object Signing and Encryption)

**COSE** stands for **CBOR Object Signing and Encryption**. It is a protocol designed to provide **security services**—like signing, encryption, and message authentication—**using the CBOR data format**.

It’s defined in [RFC 9052](https://datatracker.ietf.org/doc/html/rfc9052) and is conceptually similar to **JOSE** (JSON Object Signing and Encryption), which is used in systems like JWT.

### What Does COSE Do?

COSE allows you to apply security mechanisms to CBOR-encoded data:

- ✅ **Signing** (authentication and integrity)
- ✅ **Encryption** (confidentiality)
- ✅ **Message Authentication Codes (MAC)** (integrity + authenticity)
- ✅ **Key Distribution** (sharing cryptographic keys securely)

It can protect both the **payload** and **metadata** using a compact, CBOR-based structure.

### Main COSE Message Types

| Message Type    | Purpose                                    | RFC Section |
| --------------- | ------------------------------------------ | ----------- |
| `COSE_Sign`     | Signed message with multiple signers       | §4          |
| `COSE_Sign1`    | Signed message with a single signer        | §4.2        |
| `COSE_Encrypt`  | Encrypted message with multiple recipients | §5          |
| `COSE_Encrypt0` | Encrypted message for one recipient        | §5.2        |
| `COSE_Mac`      | Message with a MAC and multiple recipients | §6          |
| `COSE_Mac0`     | Message with a single MAC recipient        | §6.2        |
| `COSE_Key`      | Structure to represent public/private keys | §7          |

### COSE vs JOSE

| Feature             | COSE                                          | JOSE               |
| ------------------- | --------------------------------------------- | ------------------ |
| Format              | CBOR (binary)                                 | JSON (text)        |
| Compactness         | More compact (better for constrained devices) | More verbose       |
| Used in             | IoT, constrained networks                     | Web, API, JWTs     |
| Security Mechanisms | Sign, Encrypt, MAC                            | Sign, Encrypt, MAC |

### Example Use Cases

- **IoT Devices**: Secure transmission of sensor data
- **CoAP Protocol**: Used as the security layer for constrained application protocol
- **Encrypted messaging**
- **Access tokens** (alternative to JWT)

### Libraries Supporting COSE

| Language   | Library           | Notes                                           |
| ---------- | ----------------- | ----------------------------------------------- |
| Python     | `cose` (`pycose`) | Actively maintained, supports all message types |
| JavaScript | `node-cose`       | Useful in constrained JS environments           |
| Rust       | `cose` crate      | Lightweight, secure implementations             |
| C          | `cose-c`          | Reference implementation from IETF group        |
