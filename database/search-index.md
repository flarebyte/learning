# Search index

## 🔍 What Is a Free-Text Search Index?

A **free-text search index** allows users to search unstructured text (like articles, documents, logs, etc.) using arbitrary queries. Think: how search works in Google Docs, Jira, or even Stack Overflow.

Instead of scanning the entire dataset every time (which would be slow), we build an **index** that makes searches fast and flexible.

## Underlying Data Structures

### Inverted Index (Core Structure)

This is the heart of most text search engines.

- **What it does:** Maps each term (word) to a list of documents (or positions) where it appears.
- **Structure:**
  ```json
  {
    "engineer": [doc1, doc3],
    "search": [doc1, doc2],
    "index": [doc1, doc2, doc3]
  }
  ```
- Often includes positions, term frequency, etc.

### Tokenizer & Analyzer Pipeline

Before building the index, text is preprocessed:

- **Tokenizer** breaks text into words.
- **Normalizer** lowercases, removes punctuation, etc.
- **Stemming/Lemmatization** reduces words to their root form (e.g., "searching" → "search").
- **Stop Word Removal** gets rid of common words (like “the”, “and”).

### Document Store

Stores the raw or semi-structured documents so you can retrieve full results (titles, snippets, etc.).

### **Optional: Trie, FST (Finite State Transducers)**

Used for **autocomplete**, **prefix/suffix queries**, or **fuzzy matching** (e.g., Elasticsearch does this).

## 🔐 Security Considerations

### MVP-level (Minimal)

- Basic access control: Only let authenticated users search.
- Maybe field-based filtering (e.g., restrict to your own documents).

### 🛡️ Advanced

- **Row-level security**: Only show search results user is authorized to see.
- **Field-level redaction**: Hide parts of documents based on user role.
- **Multi-tenancy**: Keep tenants' data separate, especially in SaaS contexts.
- **Audit Logging**: Track what users searched for and what was returned.

## 🌍 Language & Locale Support

### Basic (MVP)

- Tokenizer for English, basic stemming.

### Advanced

- Support for dozens of languages with:
  - Locale-aware tokenization
  - Language-specific stemming
  - Synonyms & multilingual queries
  - Unicode normalization
  - Language detection per document/query

### NLP integration:

- Named entity recognition
- Semantic search (vector embeddings)
- Query expansion using ontologies

## MVP-Level Free-Text Search

**Tech stack**: SQLite + a simple tokenizer + inverted index

Features:

- Tokenization
- Basic inverted index
- Stop word removal
- Simple matching (AND/OR)

Great for: local apps, logs, small tools, notebooks.

## Full-Featured Search Engine (e.g., Elasticsearch, Solr, OpenSearch)

Includes:

- Distributed indexing
- Full-text + structured querying
- Scoring & ranking (TF-IDF, BM25)
- Faceting, filters, aggregations
- Shards, replicas, fault tolerance
- Fuzzy, wildcard, regex, proximity queries
- Query DSL
- Role-based access, field-level security
- Vector-based semantic search
- Relevance tuning (boosting, synonyms, learning-to-rank)

## When to Roll Your Own vs. Use a Tool

| Use Case                  | Build Your Own | Use Existing (Elasticsearch, Meilisearch, Typesense) |
| ------------------------- | -------------- | ---------------------------------------------------- |
| MVP, simple search        | ✅             | ❌ (overkill)                                        |
| Fast dev, rich features   | ❌             | ✅                                                   |
| Heavy customization       | ✅             | Maybe                                                |
| Large-scale, multi-tenant | ❌             | ✅                                                   |
| NLP/Semantic search       | ❌             | ✅ or combine with vector DB                         |

## Summary table

| Component        | MVP            | Advanced                             |
| ---------------- | -------------- | ------------------------------------ |
| Data Structure   | Inverted Index | Inverted Index + Trie/FST + Vectors  |
| Tokenization     | Basic split    | Locale-aware, stemming, synonyms     |
| Language Support | English only   | Multilingual, semantic search        |
| Security         | Basic auth     | Row-level ACLs, multi-tenancy        |
| Features         | Term search    | Ranking, fuzzy, autocomplete, facets |

## Side-by-Side Comparison

| Feature                | MVP Search Engine            | OpenSearch                              |
| ---------------------- | ---------------------------- | --------------------------------------- |
| Memory usage           | ~100–300 MB                  | Min 2–4 GB (can go 8+ GB easily)        |
| CPU usage (10 QPS)     | Very low (<10% of 1 core)    | Moderate (depends on scoring, filters)  |
| Storage footprint      | Tiny (flat files / SQLite)   | Large (index + segment files, replicas) |
| Complexity             | Very low                     | High (JVM tuning, config, sharding)     |
| Cost (infra)           | $0–$10/mo                    | $30–$300+/mo                            |
| Cold start / boot time | Seconds                      | ~20–60s JVM warm-up                     |
| Throughput (raw)       | Low-medium                   | High (scales with nodes)                |
| Best for               | Local tools, CLI, small SaaS | Production SaaS, enterprise search      |
