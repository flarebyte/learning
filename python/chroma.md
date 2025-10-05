# Chroma

**Chroma** is an open-source **vector database** designed for building applications that use **embeddings** — such as semantic search, recommendation systems, or retrieval-augmented generation (RAG) with large language models.

Here’s a quick overview:

- **Purpose:** Chroma stores and manages high-dimensional vector embeddings (e.g., text, images, code) so you can efficiently search for similar items using cosine or Euclidean similarity.

- **Python Integration:** It’s written in Python and has a very simple API, making it easy to integrate into machine learning or LLM workflows.

- **Core Features:**

  - Add and query embeddings with metadata and documents.
  - Fast similarity search (k-NN).
  - Persistence (can run in-memory or store data locally/on disk).
  - Integrations with frameworks like **LangChain** and **LlamaIndex** for RAG pipelines.
