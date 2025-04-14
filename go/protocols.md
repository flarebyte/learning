# Protocols

Markdown table including a Security column — highlighting each protocol/format's general security characteristics, such as schema enforcement, attack surface, and resistance to injection or fuzzing.

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
