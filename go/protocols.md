# Protocols

Table including a Security column — highlighting each protocol/format's general security characteristics, such as schema enforcement, attack surface, and resistance to injection or fuzzing.

| Protocol / Format               | Description                                     | Native / External                                | Security Characteristics                                      |
| ------------------------------- | ----------------------------------------------- | ------------------------------------------------ | ------------------------------------------------------------- |
| **Protocol Buffers (Protobuf)** | Compact, schema-based binary format from Google | 🔹 External (`protoc-gen-go`)                    | ✅ Strong schema enforcement, low attack surface              |
| **gRPC**                        | RPC over HTTP/2 using Protobuf                  | 🔹 External (`google.golang.org/grpc`)           | ✅ Secure by design, supports TLS, input strongly typed       |
| **JSON**                        | Human-readable, ubiquitous format               | ✅ Native (`encoding/json`)                      | ⚠️ Prone to injection, lacks schema by default                |
| **XML**                         | Markup language for structured data             | ✅ Native (`encoding/xml`)                       | ⚠️ Vulnerable to XXE, needs strict parsing                    |
| **YAML**                        | Config-friendly superset of JSON                | 🔹 External (`gopkg.in/yaml.v3`)                 | ⚠️ Can execute code in unsafe parsers, avoid with strict mode |
| **MessagePack**                 | Compact binary JSON alternative                 | 🔹 External (`github.com/vmihailenco/msgpack`)   | ✅ Smaller surface, but validate input strictly               |
| **CBOR**                        | Compact binary for IoT                          | 🔹 External (`github.com/fxamacker/cbor`)        | ✅ RFC-compliant, good validation features available          |
| **Cap’n Proto**                 | Very fast, schema-driven binary format          | 🔹 External (`capnproto.org/go`)                 | ✅ Strong safety guarantees, minimal parsing overhead         |
| **FlatBuffers**                 | Zero-copy serialization, great for games        | 🔹 External (`github.com/google/flatbuffers/go`) | ✅ Efficient and safe if schema is enforced properly          |
| **Thrift**                      | RPC + serialization framework                   | 🔹 External (`github.com/apache/thrift`)         | ✅ Typed and structured, depends on usage                     |
| **Avro**                        | Big data-oriented schema-driven format          | 🔹 External (`github.com/hamba/avro`)            | ✅ Secure if schema validated, used in Kafka pipelines        |
| **TOML**                        | Simple config format (used in Go modules)       | 🔹 External (`github.com/BurntSushi/toml`)       | ✅ Safe for configs, no execution                             |
| **INI**                         | Legacy config format                            | 🔹 External (`gopkg.in/ini.v1`)                  | ⚠️ Minimal validation, watch for injection in values          |

## 🔐 Security Highlights:

- ✅ **Protobuf/gRPC/Cap’n Proto/FlatBuffers** → safest for structured, binary communication.
- ⚠️ **JSON/YAML/XML/INI** → require care to sanitize inputs and validate schema manually.
- ✅ **CBOR/MessagePack** → compact and safe if parsers are strict and inputs validated.

## JSON vs CBOR

| Feature                  | CBOR                          | JSON                     |
| ------------------------ | ----------------------------- | ------------------------ |
| Format                   | Binary                        | Text                     |
| Schema Enforcement       | ✅ Better type control        | ❌ None by default       |
| Injection Risk           | ✅ Low (hard to exploit)      | ⚠️ High (text-based)     |
| Canonicalization         | ✅ RFC-backed                 | ❌ No standard support   |
| Parser Efficiency        | ✅ High (compact & fast)      | ⚠️ Varies (can be slow)  |
| Secure Signature Support | ✅ Easier with canonical form | ⚠️ Requires custom logic |

## AWS Lambda in Go (backend) ↔ Flutter (all platforms, Dart)

| Approach                   | Cross-platform   | Performance  | Simplicity            | Security             | Infra Needs     |
| -------------------------- | ---------------- | ------------ | --------------------- | -------------------- | --------------- |
| **REST + JSON**            | ✅✅✅           | ⚠️ Medium    | ✅ Easy               | ✅ With HTTPS/Auth   | ✅ Minimal      |
| **REST + Protobuf**        | ✅✅✅           | ✅ High      | ✅ Moderate           | ✅ Strong with HTTPS | ✅ Minimal      |
| **gRPC + Protobuf**        | ⚠️ gRPC-Web only | ✅✅ High    | ⚠️ Complex            | ✅✅ Strong          | ⚠️ Envoy/Proxy  |
| **CBOR/FlatBuffers**       | ⚠️ Dart limited  | ✅ High      | ❌ Niche              | ✅ Yes               | ❌ Complex      |
| **WebSocket (JSON/Proto)** | ✅               | ✅ Real-time | ⚠️ Tricky with Lambda | ✅ Depends on setup  | ⚠️ AWS-specific |

> Use **Protobuf over standard REST (HTTPS)** for:
>
> - ✅ Compact, schema-safe communication
> - ✅ Full compatibility with Flutter on all platforms
> - ✅ Simple deployment using AWS Lambda + API Gateway

## Amazon API Gateway (HTTP API or REST API)

→ **Both support binary payloads** like Protobuf, with a few extra steps.

> ✅ **Use API Gateway HTTP API** (simpler, modern)  
> ✅ **Set `Content-Type: application/x-protobuf`**  
> ✅ **Binary data flows cleanly** between Flutter and Go Lambda  
> 🔁 If using REST API, you must enable Binary Media Types and handle Base64

## Options Breakdown

| API Gateway Type  | Supports Protobuf     | Simplicity                | Best For                          |
| ----------------- | --------------------- | ------------------------- | --------------------------------- |
| **HTTP API** (v2) | ✅ Yes (with config)  | ✅ Simpler setup          | Modern REST APIs (preferred)      |
| **REST API** (v1) | ✅ Yes (more options) | ⚠️ More complex           | Fine-grained control, legacy use  |
| **WebSocket API** | ⚠️ Not suitable       | ❌ No binary body support | Real-time, not ideal for Protobuf |

## How to Make Protobuf Work

**Enable Binary Media Types**

- By default, API Gateway assumes text payloads.
- You need to **tell it to allow binary content**, like `application/x-protobuf`.

**Go Lambda Handler**

- Use `APIGatewayProxyRequest.Body` and decode from binary.
- If using REST API, you'll **Base64-decode the body** (if `isBase64Encoded == true`).
- Decode the Protobuf message using generated Go code.

**Client-Side (Flutter/Dart)**

- Send request using HTTP POST/GET
- Set headers:
  ```http
  Content-Type: application/x-protobuf
  Accept: application/x-protobuf
  ```
- Body should be a **raw Protobuf-encoded binary**.

**Securing It**

- Use **IAM, JWT (via Cognito), or API keys** with API Gateway for auth.
- Always use **HTTPS** (automatically enforced by API Gateway).

**For REST API (v1):**

- In settings, **add `application/x-protobuf`** (or your custom MIME type) to **Binary Media Types**.
- You’ll also need to **Base64 encode** the request/response body in Lambda.

**For HTTP API (v2):**

- It **automatically supports binary** when the `Content-Type` is a binary MIME type — you don’t need to define binary media types manually.
- Still, make sure the **client and server both respect `Content-Type: application/x-protobuf`**.
