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

## Inverted Index – Beyond Just Word → Document

At its simplest, an inverted index maps **terms (words)** to **documents** that contain them.  
But for **ranking, relevance, and advanced queries**, we often store **extra info**:

### Positions

- **What it is:** Where exactly the term appears in the document.
- **Example:**
  ```
  "search index engine" → "index" is the 2nd word
  ```
- **Why it matters:**
  - Enables **phrase search**: `"search index"` matches only if "search" is followed by "index".
  - Enables **proximity search**: find words near each other (e.g., `"engine search"~3`).

### Term Frequency (TF)

- **What it is:** How many times a word appears in a document.
- **Example:**
  ```
  Doc1: "search search engine" → TF("search") = 2
  ```
- **Why it matters:**
  - Higher frequency often means higher **relevance**.
  - Used in ranking formulas like **TF-IDF** or **BM25**.

### Inverse Document Frequency (IDF)

- Not stored per document, but computed globally.
- Rare words (like “neural”) get more weight than common ones (like “the”).

### What It Looks Like (Conceptually)

```json
"search": [
  { "doc": 1, "positions": [0, 2], "tf": 2 },
  { "doc": 2, "positions": [4], "tf": 1 }
]
```

| Metadata           | Purpose                     |
| ------------------ | --------------------------- |
| **Position**       | Phrase/proximity search     |
| **Term Frequency** | Relevance scoring           |
| **Doc Frequency**  | Global rarity (used in IDF) |

## What Are Ranking Formulas?

When you search for `"open source engine"`, multiple documents might match.

Ranking formulas **score each document** based on how relevant it is to the query — so you can show the best matches first.

They work using features like:

- How often terms appear
- How rare terms are across the corpus
- Document length (shorter might be better)
- Exact vs fuzzy matches

## TF-IDF (Term Frequency – Inverse Document Frequency)

### Intuition

- Words that appear **a lot** in a document = likely important (**TF**)
- But if they appear **everywhere**, they aren’t helpful (**IDF** penalizes common words)

### Formula

For a term **t** in a document **d**:

```
TF-IDF(t, d) = TF(t, d) * IDF(t)
```

Where:

- **TF(t, d)** = raw count of term **t** in doc **d** (or log-scaled)
- **IDF(t)** = `log(N / DF(t))`
  - N = total number of documents
  - DF(t) = number of docs where **t** appears

### Example:

- `"engine"` appears **10 times** in doc A, and **1000 out of 10,000 docs**.
- `"the"` appears **50 times** in doc A, but in **all 10,000 docs**.

Then:

- `TF-IDF("engine", A)` is **high**
- `TF-IDF("the", A)` is **low** (because it's in every doc → low IDF)

### ✅ Pros

- Simple, intuitive
- Great for early prototyping

### ❌ Cons

- Doesn’t handle document length normalization well
- Sensitive to exact word form (doesn’t know "search" ≈ "searching")

## BM25 (Best Matching 25)

BM25 is a **probabilistic ranking model**, widely used in real-world search engines (Lucene, Elasticsearch, Solr).

### Intuition

- Builds on TF-IDF
- Adds **smart normalization**:
  - Penalizes long documents
  - Dampens overly repeated terms

### Formula (simplified)

For term **t** in query **q**, in doc **d**:

```
Score(d, q) = Σ over t ∈ q:
    IDF(t) * ((TF(t, d) * (k + 1)) / (TF(t, d) + k * (1 - b + b * (|d| / avg_dlen))))
```

Where:

- **TF(t, d)** = term frequency in doc
- **|d|** = length of doc
- **avg_dlen** = average doc length
- **k**, **b** = tuning constants:
  - `k` (term saturation) ≈ 1.2–2.0
  - `b` (length normalization) ≈ 0.75

### ✅ Pros

- State-of-the-art for keyword search
- Tunable and robust
- Handles long vs short docs better than TF-IDF

### ❌ Cons

- Still bag-of-words (no word order/semantics)
- Needs tuning for best results

## TF-IDF vs BM25 – Quick Comparison

| Feature              | TF-IDF                 | BM25                                |
| -------------------- | ---------------------- | ----------------------------------- |
| Simplicity           | ✅ Very simple         | ❌ More complex                     |
| Length normalization | ❌ Poor                | ✅ Tuned with `b`                   |
| Saturation control   | ❌ None                | ✅ Handled via `k`                  |
| Popularity           | Good for small engines | ✅ Default in Lucene, Elasticsearch |
| Scoring Quality      | Medium                 | ✅ High (better relevance)          |

## Beyond BM25?

If you go further:

- **BM25+**, **BM25L**: BM25 tweaks
- **Learning-to-rank (LTR)**: ML-based ranking (e.g., LambdaMART)
- **Dense vector models**: Semantic search using embeddings (BERT, OpenAI embeddings)

## What Is a Tokenizer?

A **tokenizer** is the first step in processing text for search.

### What It Does:

- Takes **raw text** and splits it into **tokens** (words or terms).
- Optionally, performs normalization like lowercasing or removing punctuation.

### Example

Raw text:

```
"OpenSearch is awesome! Search engines love it."
```

Basic tokenization might output:

```
["opensearch", "is", "awesome", "search", "engines", "love", "it"]
```

More advanced tokenization could also:

- **Lowercase** everything
- **Remove stop words** (like "is", "it")
- **Stemming/Lemmatization**: "engines" → "engine"

## Common Tokenizer Types

| Type               | Description                                            |
| ------------------ | ------------------------------------------------------ |
| **Whitespace**     | Splits on spaces                                       |
| **Word**           | Splits on word boundaries (excludes punctuation)       |
| **N-gram**         | Creates overlapping chunks (e.g., "sea", "ear", "arc") |
| **Regex-based**    | Fully custom rules (e.g., `\w+`)                       |
| **Language-aware** | Handles grammar, accents, compound words               |

## When to Use What?

| Language | Use Case                         | Library                             |
| -------- | -------------------------------- | ----------------------------------- |
| JS/TS    | Browser-based search / NLP tools | `natural`, `wink-tokenizer`, `lunr` |
| Go       | Building search/index engines    | `bleve`, `segment`, `gojieba`       |

| Tokenizer Type         | Returns Positions?            | Returns Frequency?        |
| ---------------------- | ----------------------------- | ------------------------- |
| `natural` (JS)         | ❌                            | ❌ (but easy to add)      |
| `wink-tokenizer` (JS)  | ✅ `offset`, `length`         | ❌                        |
| `segment` (Go)         | ✅                            | ❌                        |
| `bleve` analyzers (Go) | ✅ (as part of full pipeline) | ✅ (used during indexing) |

In systems like **Lucene**, **Elasticsearch**, or **Bleve**:

- The tokenizer is part of an **analyzer chain**.
- It produces tokens with:
  - **term**
  - **position**
  - **start & end char offsets**
  - **payload** (optional)
- Frequency is **computed** during indexing (count of how often a term appears per document).

| Feature       | Tokenizer                   | Indexing Stage |
| ------------- | --------------------------- | -------------- |
| **Tokens**    | ✅                          | ✅             |
| **Position**  | Sometimes ✅                | ✅             |
| **Frequency** | ❌ (usually computed later) | ✅             |
