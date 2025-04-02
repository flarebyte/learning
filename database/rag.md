# Retrieval-Augmented Generation (RAG)

**What is it?**

RAG is a method that combines **retrieval of external knowledge** with **language generation**. Instead of relying solely on the model's internal knowledge, RAG **retrieves documents** from a knowledge base and **generates answers** using both the prompt and the retrieved data.

#### Under the Hood

- A **retriever** (e.g., dense vector retriever like FAISS or BM25) fetches top-k documents related to the query.
- A **generator** (e.g., a transformer like BART, T5, or GPT) uses the retrieved content along with the input query to generate a final response.
- Original RAG (by Facebook AI) used DPR (Dense Passage Retrieval) for retrieval and BART for generation.

#### ✅ Pros

- **Up-to-date knowledge**: Can access external sources without retraining the LLM.
- **Interpretability**: You can inspect retrieved documents to understand why the model gave an answer.
- **Efficiency**: Requires less training data because knowledge lives outside the model.

#### ❌ Cons

- **Latency**: Two-step process (retrieve → generate) can be slower.
- **Retrieval quality bottleneck**: Garbage in, garbage out.
- **Complexity**: More moving parts — you need to manage a retriever and a generator.

#### Main Actors

- **Meta (Facebook)** – Original RAG paper.
- **OpenAI** – Retrieval plugins, `retrieval` tools in GPT.
- **LangChain**, **Haystack**, **LlamaIndex** – Popular frameworks for building RAG pipelines.
- **Pinecone**, **Weaviate**, **Qdrant**, **Vespa**, **Redis**, **FAISS** – Vector database support for RAG.

## Hybrid Retrieval

**What is it?**  
Hybrid retrieval combines **dense retrieval** (semantic search using embeddings) and **sparse retrieval** (traditional keyword search like BM25). This ensures robustness in understanding meaning _and_ matching important terms.

#### Under the Hood

- Run two retrieval strategies:
  - **Sparse**: Uses inverted index (BM25/TF-IDF).
  - **Dense**: Uses vector search (e.g., cosine similarity of embeddings).
- Results from both are combined, often re-ranked using a scoring mechanism.

#### ✅ Pros

- **Best of both worlds**: Captures semantics and exact matches.
- **Resilient**: Works well on both keyword-heavy and natural language queries.
- **Improved recall**: Covers cases where either approach would fail alone.

#### ❌ Cons

- **Slower**: Running two searches increases latency.
- **Complex ranking**: Combining results and tuning scores adds complexity.
- **Requires multiple systems** (e.g., Elasticsearch + FAISS).

#### Main Actors

- **Elastic**, **OpenSearch** – Hybrid search using BM25 + dense vector support.
- **Cohere**, **Vespa**, **Weaviate**, **Qdrant**, **Pinecone** – Offer hybrid capabilities.
- **Haystack**, **LlamaIndex**, **LangChain** – Frameworks that support hybrid retrieval pipelines.

## Multimodal or Multi-Index Retrieval

**What is it?**  
These systems can retrieve and reason over **multiple data modalities** (e.g., text, image, video, code) or **multiple indices** (structured, unstructured, tabular, etc.).

#### 🔧 Under the Hood

- Each modality or index has:
  - **A specialized encoder** (e.g., CLIP for images, CodeBERT for code).
  - **Its own vector index** (often using FAISS, Qdrant, or similar).
- A **query** is routed to appropriate modality-specific retrievers.
- Results are **merged or ranked** for downstream generation or display.

#### ✅ Pros

- **Rich interaction**: Works across types of content (text-to-image, text-to-table, etc.).
- **Powerful UX**: Users can input in one modality and get output from another.
- **Flexible architecture**: Data silos can stay separate but still be accessed together.

#### ❌ Cons

- **Complex architecture**: Need multiple encoders, indexers, retrievers.
- **Hard to rank**: Comparing relevance across modalities is tricky.
- **Training challenges**: Multimodal embedding spaces are harder to align.

#### Main Actors

- **OpenAI (GPT-4V)** – Multimodal model with text/image input.
- **Google DeepMind (Gemini)** – Designed to handle multimodal input natively.
- **CLIP**, **BLIP**, **Flamingo** – Vision-language pre-trained models.
- **LlamaIndex**, **LangChain** – Tools supporting multi-index and multimodal retrieval.
- **Hugging Face** – Hosts many vision-language and multimodal models.

### Summary Table

| Feature          | RAG                     | Hybrid Retrieval  | Multimodal / Multi-Index       |
| ---------------- | ----------------------- | ----------------- | ------------------------------ |
| Retrieval Method | Dense                   | Dense + Sparse    | Cross-modality / multi-source  |
| Generation Step  | Yes                     | Optional          | Optional (depends on use case) |
| Main Benefit     | Up-to-date + contextual | Robust matching   | Rich, flexible interaction     |
| Complexity       | Medium                  | Medium-High       | High                           |
| Popular Tools    | RAG (Meta), LangChain   | Haystack, Elastic | LlamaIndex, CLIP, GPT-4V       |
