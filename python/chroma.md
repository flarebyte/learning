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

Here’s a **Chroma (embedded) API quicksheet**—concise purpose + bite-size examples for the calls you’ll use most.

# Setup

```python
# install
# pip install chromadb

import chromadb
from chromadb.config import Settings

# In-memory (ephemeral)
client = chromadb.Client()

# On-disk (persistent)
from chromadb import PersistentClient
client = PersistentClient(path="./chroma-data")
```

---

# Client: collections (lifecycle)

### `create_collection(name, metadata=None, embedding_function=None)`

Create a new collection.

```python
collection = client.create_collection(
    name="docs",
    metadata={"topic": "guides"}
)
```

### `get_or_create_collection(name, **kwargs)`

Return existing or create if missing (idempotent).

```python
collection = client.get_or_create_collection("docs")
```

### `get_collection(name)`

Fetch a collection by name.

```python
collection = client.get_collection("docs")
```

### `list_collections()`

List all collections.

```python
collections = client.list_collections()
```

### `delete_collection(name)`

Drop a collection and its data.

```python
client.delete_collection("docs")
```

---

# Collection: write / mutate

### `add(ids, documents=None, embeddings=None, metadatas=None)`

Insert new records (vectors). Supply `documents` (auto-embed if you configured an embedding function) **or** precomputed `embeddings`.

```python
collection.add(
    ids=["1","2"],
    documents=["Chroma is a vector DB", "It supports similarity search"],
    metadatas=[{"source":"intro"}, {"source":"intro"}]
)
```

### `upsert(ids, documents=None, embeddings=None, metadatas=None)`

Insert or replace by id (create if new, update if exists).

```python
collection.upsert(
    ids=["2","3"],
    documents=["(updated) It supports fast k-NN", "It’s great for RAG"]
)
```

### `update(ids, documents=None, embeddings=None, metadatas=None)`

Update existing records by id (fails if an id doesn’t exist).

```python
collection.update(
    ids=["1"],
    metadatas=[{"source":"quickstart", "tag":"edited"}]
)
```

---

# Collection: read / query

### `get(ids=None, where=None, where_document=None, limit=None, offset=None, include=None)`

Fetch records by ids or filters. `include` controls returned fields: `["embeddings","documents","metadatas","distances"]`.

```python
rows = collection.get(
    where={"source":"intro"},
    include=["documents","metadatas"]
)
```

### `query(query_texts=None, query_embeddings=None, n_results=5, where=None, where_document=None, include=None)`

Similarity search over your vectors.

```python
res = collection.query(
    query_texts=["What is Chroma?"],
    n_results=3,
    include=["documents","metadatas","distances"]
)
```

### `peek(limit=10)`

Quick glance at the first N items.

```python
sample = collection.peek(5)
```

### `count()`

Number of items in the collection.

```python
num = collection.count()
```

---

# Collection: delete / manage

### `delete(ids=None, where=None, where_document=None)`

Remove records by ids or filters.

```python
collection.delete(where={"source":"intro"})
```

### `modify(name=None, metadata=None)`

Rename or update collection metadata.

```python
collection.modify(name="docs_v2", metadata={"topic":"guides-v2"})
```

---

# Filtering cheatsheet

- `where`: filter by metadata (exact/relational ops depending on your data).

  ```python
  where={"author":"alice"}               # equals
  ```

- `where_document`: filter by document text (simple contains / match behavior).

  ```python
  where_document={"$contains":"Chroma"}
  ```

You can combine filters with `query()` or `get()`.

---

# Utilities

### `client.reset()`

Delete **all** data managed by this client (dangerous).

```python
client.reset()
```

### `client.heartbeat()` / `client.version()`

Health check and version string.

```python
client.heartbeat()
client.version()
```

---

# Optional: embedding functions (auto-embed)

Provide an embedding function so `add()`/`upsert()` can take plain texts.

```python
from chromadb.utils import embedding_functions
ef = embedding_functions.SentenceTransformerEmbeddingFunction(model_name="all-MiniLM-L6-v2")

collection = client.get_or_create_collection("docs", embedding_function=ef)
collection.add(ids=["1"], documents=["Chroma makes vector search easy"])
```

---

# Minimal end-to-end example

```python
import chromadb
client = chromadb.Client()
col = client.get_or_create_collection("faq")

col.add(
    ids=["a","b","c"],
    documents=[
        "Chroma is an embedded or server vector database.",
        "Use collections to store your embeddings and metadata.",
        "Query finds the most similar documents by cosine distance."
    ],
    metadatas=[{"cat":"about"},{"cat":"howto"},{"cat":"search"}]
)

print("Items:", col.count())

ans = col.query(
    query_texts=["How do I search in Chroma?"],
    n_results=2,
    include=["documents","metadatas","distances"]
)
print(ans["documents"][0])
```
