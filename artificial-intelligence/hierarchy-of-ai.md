# The Hierarchy of AI by System Architecture

Artificial intelligence systems can be understood as a **progressive hierarchy of capability and autonomy** — from simple models that generate predictions to autonomous systems that plan, reason, and act independently.
Each level builds upon the previous one, adding new components like memory, retrieval, or tool use to enhance intelligence and adaptability.

---

### ⚙️ AI Hierarchy by Architecture / System Design

| **Level** | **Name**                                        | **Description**                                                                                                                                                                     | **Popular Tools / Examples**                                                                                                                          |
| --------- | ----------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------- |
| **A**     | **Model**                                       | A standalone trained neural network that generates predictions (e.g., text, image, or sound) based solely on its internal parameters. No external memory or knowledge retrieval.    | **GPT-4 / GPT-5 (OpenAI)**, **Claude 3 (Anthropic)**, **Gemini (Google DeepMind)**, **Mistral**, **LLaMA 3 (Meta)**                                   |
| **B**     | **RAG System (Retrieval-Augmented Generation)** | Extends a base model by connecting it to external data sources (e.g., documents, APIs, vector databases). Enhances factual accuracy and access to up-to-date info.                  | **ChatGPT with “memory” or “file retrieval”**, **LangChain RAG**, **LlamaIndex**, **Haystack**, **Perplexity AI**                                     |
| **C**     | **Agent**                                       | Adds reasoning, planning, and tool use. The model can decide _what actions to take_, _which tools to use_, and _how to achieve a goal_ step-by-step.                                | **OpenAI o1 & GPT-5 with agents**, **LangChain Agents**, **CrewAI**, **AutoGPT**, **BabyAGI**, **Claude with Tools**, **Devin (Cognition AI)**        |
| **D**     | **Multi-Agent System**                          | A collection of specialized agents that collaborate or compete to complete complex tasks. Each agent can have distinct roles (researcher, coder, tester, etc.).                     | **CrewAI multi-agent framework**, **Meta’s AgentSociety**, **AutoGen (Microsoft)**, **ChatDev**, **HuggingGPT**                                       |
| **E**     | **Autonomous Cognitive System**                 | Agents with long-term memory, persistent goals, and self-improvement capabilities. They can reflect, learn, and operate semi-independently over time — edging toward **proto-AGI**. | **OpenAI o1-preview (reasoning model)**, **Devin (software engineer AI)**, **PI (Inflection AI)**, **Self-operating AutoGPT**, **Kindred AI (early)** |

---

### In Short

> **Model → RAG → Agent → Multi-Agent → Autonomous Cognitive System**

Each stage adds a layer of intelligence and autonomy:

- **Models** generate.
- **RAGs** know.
- **Agents** act.
- **Multi-agents** collaborate.
- **Autonomous systems** _think long-term_.

## What Are Planning Algorithms in LLM Agents?

> **Planning algorithms** are _control strategies_ that sit **outside** the language model and determine how to use its reasoning over multiple steps.

They define **how the agent breaks down a goal**, **chooses actions**, and **adapts based on results**.

Think of them as the **cognitive architecture** of an agent — deciding _how it reasons_ and _when to act_.

---

## The Core Anatomy of a Planning Algorithm

Every planning algorithm (ReAct, ToT, etc.) typically has these building blocks:

| Component                      | Description                                          |
| ------------------------------ | ---------------------------------------------------- |
| **Goal Interpreter**           | Understands the user’s intent or objective           |
| **Reasoning Engine (the LLM)** | Generates thoughts, hypotheses, or subgoals          |
| **Planner / Controller**       | Decides which step to take next (based on reasoning) |
| **Executor**                   | Performs the action (e.g., run tool, call API)       |
| **Evaluator / Reflector**      | Judges if the outcome is correct or needs revision   |
| **Memory / Context Manager**   | Stores intermediate results or lessons for reuse     |

Different algorithms combine these pieces in unique ways.

| Algorithm                  | Key Mechanism                           | Pros                           | Cons                          | Example Systems         |
| -------------------------- | --------------------------------------- | ------------------------------ | ----------------------------- | ----------------------- |
| **Chain-of-Thought (CoT)** | Linear step-by-step reasoning           | Simple, powerful               | No actions or feedback        | GPT-4, Claude 3         |
| **ReAct**                  | Thought–Action–Observation loop         | Tool use, iterative correction | Can loop indefinitely         | LangChain, AutoGPT      |
| **Tree-of-Thought**        | Branching reasoning paths               | Explores alternatives          | Computationally heavy         | DeepMind ToT, o1 models |
| **Reflexion**              | Self-evaluation after actions           | Learns from mistakes           | Needs strong evaluation logic | AutoGPT v2, CrewAI      |
| **Plan-and-Execute**       | High-level planning + subtask execution | Structured and stable          | Rigid, less adaptive          | LangChain, OpenDevin    |
| **Graph-of-Thought**       | Multi-agent graph reasoning             | Parallel, collaborative        | Complex orchestration         | AutoGen, ChatDev        |
