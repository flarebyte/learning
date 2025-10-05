# CUDA

## CUDA + Data Science Access by Language

| Language        | CUDA Access  | Data Science Ecosystem | Notes                                         |
| --------------- | ------------ | ---------------------- | --------------------------------------------- |
| **Python**      | ✅ Yes       | ✅ Massive             | Best all-around for DS + GPU                  |
| **C / C++**     | ✅ Yes       | ⚠️ Medium              | Native CUDA access, high performance          |
| **Rust**        | ⚠️ Yes-ish   | ⚠️ Growing             | Crates exist, performance solid               |
| **Go**          | ⚠️ Limited   | ⚠️ Light               | No native CUDA; some bindings, but early      |
| **TypeScript**  | ❌ No        | ⚠️ Good (via JS)       | Access via JavaScript ML libs, not GPU-native |
| **Haskell**     | ⚠️ Limited   | ⚠️ Limited             | Experimental support only                     |
| **Lisp/Racket** | ❌ Rare      | ❌ Minimal             | Not suited for DS/GPU out of the box          |
| **Java/Kotlin** | ⚠️ Yes-ish   | ✅ Good (esp. Java)    | ND4J, JCuda; Kotlin can wrap these            |
| **Scala**       | ✅ (via JVM) | ✅ Yes (Spark, Breeze) | Great for big data & Spark jobs               |

## Details per Language

### **Python**

- **CUDA**: Use `CuPy`, `numba.cuda`, or TensorFlow with GPU.
- **DS/ML**: King of the hill – pandas, scikit-learn, PyTorch, TensorFlow, matplotlib, etc.
- **Verdict**: Best if you want quick access to data science and GPU with minimal setup.

---

### **C/C++**

- **CUDA**: Direct access. CUDA is written in C/C++.
- **DS/ML**: Not super convenient, but great for integrating with ML backends like PyTorch, TensorFlow.
- **Verdict**: Ideal for performance-critical language design with GPU features.

---

### **Rust**

- **CUDA**: Use `cust`, `rust-cuda`, or interop with C. Early stage but works.
- **DS/ML**: Crates like `ndarray`, `tch-rs` (PyTorch bindings), but not as rich as Python.
- **Verdict**: Great for building performant interpreters with GPU support, though DS is still emerging.

---

### **Haskell**

- **CUDA**: `accelerate` library offers GPGPU support, but niche.
- **DS/ML**: Sparse – some linear algebra, but not mainstream.
- **Verdict**: Elegant but not a go-to for data science or GPU unless you're deep into functional stacks.

---

### **Java/Kotlin**

- **CUDA**: Via ND4J, DeepLearning4J, or JCuda (Java bindings for CUDA).
- **DS/ML**: Decent ecosystem for JVM – Weka, Smile, DL4J.
- **Verdict**: Not as friendly as Python, but solid for JVM-based data science or deep learning.

---

### **Scala**

- **CUDA**: Through JVM interop.
- **DS/ML**: Spark MLlib, Breeze for math, and works with Java ML libraries.
- **Verdict**: Great for big data; less ideal for low-level GPU work.

### **Go (Golang)**

- **CUDA**: No native CUDA support, but some options:
  - Can call C/C++ CUDA code via `cgo`
  - Libraries like `gocudnn` or wrappers for CuDNN exist, but are niche
- **Data Science**: Early stage
  - Libraries like `gonum`, `golearn` for basic ML
  - No deep learning frameworks like PyTorch/TensorFlow
- **Verdict**: Great for concurrency and systems, not ideal for GPU/DS-heavy DSLs unless integrating with C/C++.

🔧 Use Go if:

- You want to build a fast, concurrent language runtime
- You don't need heavy ML/GPU built in

---

### **TypeScript (and JavaScript)**

- **CUDA**: ❌ No native CUDA (runs in browser or Node.js)
  - Some browser-based GPU options via WebGL or **WebGPU**
  - Libraries like [TensorFlow.js](https://www.tensorflow.org/js) can run on GPU in the browser
- **Data Science**:
  - TS/JS wrappers for ML: `TensorFlow.js`, `ml5.js`, `brain.js`, `scikit.js` (a Python-like wrapper)
  - Can do decent math, viz, ML in the browser or Node
- **Verdict**: Very solid for lightweight scripting languages with web-based ML, not ideal for low-level GPU work.

🔧 Use TypeScript if:

- You're building a language that runs in the browser
- You want portable DS/ML (especially for educational or web tools)

---

## TL;DR: Including Go & TypeScript

| Goal                                | Best Host Language         |
| ----------------------------------- | -------------------------- |
| Build a GPU-accelerated DSL         | C++, Python, Rust          |
| Rapid prototyping + DS              | Python                     |
| Language that runs in browser w/ ML | TypeScript + TensorFlow.js |
| Fast, concurrent embedded DSL       | Go                         |
| JVM-based language with DS          | Scala, Kotlin              |
| Advanced type-safe compiler tools   | Haskell, Rust              |
