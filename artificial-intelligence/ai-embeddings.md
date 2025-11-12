# AI Embeddings

## Commercial embeddings

| Provider                | Model                                           | Price per 1M input tokens (USD) | Key Features / Qualitative Notes                                                                      |
| ----------------------- | ----------------------------------------------- | ------------------------------: | ----------------------------------------------------------------------------------------------------- |
| **OpenAI**              | text-embedding-3-small                          |                       **$0.02** | Excellent value; strong general quality; 1536 dimensions; supports reduced dimensions for efficiency. |
| **OpenAI**              | text-embedding-3-large                          |                       **$0.13** | Higher accuracy; 3072 dimensions; best for semantic search and multilingual cases.                    |
| **Google (Gemini API)** | Gemini Embedding (text-embedding-004 successor) |                       **$0.15** | Top-tier multilingual performance; flexible dimensions (128–3072); high MTEB benchmark scores.        |
| **Voyage AI**           | voyage-3.5-lite                                 |                       **$0.02** | Very low cost; solid retrieval quality; generous free tier.                                           |
| **Voyage AI**           | voyage-3.5                                      |                       **$0.06** | Balanced price/performance; great for RAG and document retrieval; 32k context.                        |
| **Voyage AI**           | voyage-3-large / voyage-context-3               |                       **$0.18** | Highest quality Voyage models; context-aware embeddings for better retrieval.                         |
| **AWS Bedrock**         | Titan Text Embeddings V2                        |                       **$0.02** | Cheap, scalable; supports multiple dimensions (256/512/1024); multilingual.                           |
| **Cohere**              | Embed-4 (text)                                  |                       **$0.12** | Enterprise-grade; supports both text and image embeddings; robust retrieval tools.                    |
| **Mistral**             | Codestral Embed 2505                            |                       **$0.15** | Optimized for code retrieval; batch mode 50% cheaper; strong for developer data.                      |

## local embedding models

| Model                                     | Provider / Source      | Dim  | Quality vs OpenAI (approx.)       | Typical Hardware Need        | Strengths                                                                | Weaknesses / Notes                                  |
| ----------------------------------------- | ---------------------- | ---- | --------------------------------- | ---------------------------- | ------------------------------------------------------------------------ | --------------------------------------------------- |
| **bge-m3**                                | BAAI / Hugging Face    | 1024 | ≈ OpenAI `3-small` (85–90%)       | CPU-friendly; fine on laptop | Very strong multilingual support; compact; top performer for open-source | Slightly lower English recall than OpenAI `3-large` |
| **bge-large-en-v1.5**                     | BAAI                   | 1024 | ≈ Between `3-small` and `3-large` | Needs 6–8 GB VRAM            | Excellent English retrieval; efficient for RAG                           | Monolingual (English only)                          |
| **E5-mistral-7B-instruct**                | Mistral / Hugging Face | 4096 | ≈ `3-large`                       | Requires GPU (≥ 16 GB VRAM)  | Strong general semantic understanding; open weights                      | Higher latency; large model size                    |
| **gte-large**                             | Alibaba / Hugging Face | 1024 | ≈ `3-small`                       | Works on CPU; < 8 GB VRAM    | High efficiency; good retrieval and clustering                           | Slightly weaker multilingual support                |
| **Instructor-xl**                         | HKUNLP                 | 768  | ≈ `3-small`                       | CPU-capable (8–12 GB RAM)    | Handles “instruction-tuned” queries well (e.g., tasks, questions)        | Older; weaker on recent MTEB datasets               |
| **MiniLM-L6-v2**                          | Microsoft / SBERT      | 384  | ≈ 60–70% of `3-small`             | Very light; runs on CPU      | Great speed; ideal for prototyping and small RAG                         | Lower recall and semantic coverage                  |
| **nomic-embed-text-v1.5**                 | Nomic AI               | 768  | ≈ 80–85% of `3-small`             | CPU or modest GPU            | Tuned for retrieval; transparent open license                            | English-biased; medium recall                       |
| **paraphrase-multilingual-mpnet-base-v2** | SBERT / Hugging Face   | 768  | ≈ 75–80% of `3-small`             | CPU-friendly                 | Multilingual; very easy to use                                           | Slightly dated and smaller semantic range           |

### 🧭 Quality Benchmarks (MTEB Approximation)

| Category                   | OpenAI `3-small` | OpenAI `3-large` | Best open-source (bge-m3 / E5-Mistral) |
| -------------------------- | ---------------- | ---------------- | -------------------------------------- |
| **MTEB Avg (retrieval)**   | ~62              | ~70–72           | ~63–68                                 |
| **Multilingual retrieval** | ~65              | ~70              | ~68 (bge-m3)                           |
| **Clustering / STS**       | ~70              | ~76              | ~70–74                                 |

_(MTEB = Massive Text Embedding Benchmark; scores normalized to 0–100 for interpretability.)_

### 🧩 Key Takeaways

- 💸 **Local = free inference** once you have hardware. On CPU or small GPU, models like **bge-m3** or **gte-large** can match ~90% of OpenAI `text-embedding-3-small` accuracy.
- ⚙️ **OpenAI models** still lead in **consistency, multilingual balance, and speed per token**, especially at scale.
- 🧠 **E5-Mistral** and **bge-m3** currently top open-source leaderboards and are the best starting points for serious local RAG setups.
- 🪶 **MiniLM** and **Instructor** models are lightweight options for laptops or serverless deployments.
- 🔐 Local models ensure **full data privacy**, unlike cloud APIs.
