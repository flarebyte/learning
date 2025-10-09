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

## Planning Algorithms — Comparative Matrix

| **Algorithm**                         | **Implementation Complexity** ⚙️                                            | **Result Quality / Reasoning Power** 🧠                          | **Pros** ✅                                                                                      | **Cons** ⚠️                                                                                   | **Typical Use-Cases**                                     |
| ------------------------------------- | --------------------------------------------------------------------------- | ---------------------------------------------------------------- | ------------------------------------------------------------------------------------------------ | --------------------------------------------------------------------------------------------- | --------------------------------------------------------- |
| **1. Chain-of-Thought (CoT)**         | ⭐ _Very Low_ — just prompting (no external logic)                          | ⭐⭐ _Moderate_ — good for linear reasoning, fails on long tasks | - Easiest to implement<br>- Fast, single model call<br>- Works well for factual or step problems | - No feedback or correction<br>- No actions/tools<br>- Context-limited                        | Math, logic, factual Q&A, reasoning benchmarks            |
| **2. ReAct (Reason + Act)**           | ⭐⭐ _Low–Medium_ — simple control loop + tool registry                     | ⭐⭐⭐ _High_ — good reasoning + tool use                        | - Enables tool use & iteration<br>- Transparent reasoning trace<br>- Flexible & modular          | - Risk of infinite loops<br>- Needs controller code<br>- Can degrade with noisy tools         | Search-based tasks, RAG agents, code execution bots       |
| **3. Plan-and-Execute**               | ⭐⭐⭐ _Medium_ — requires planner + executor + task queue                  | ⭐⭐⭐ _High_ — consistent, structured reasoning                 | - Clear separation of concerns<br>- Parallelizable subtasks<br>- Stable & predictable            | - Less adaptive mid-plan<br>- Needs re-planning logic<br>- More overhead                      | Workflow automation, data pipelines, multi-step coding    |
| **4. Reflexion**                      | ⭐⭐⭐⭐ _Medium–High_ — adds evaluation + memory components                | ⭐⭐⭐⭐ _Very High_ — self-improving, fewer repeated errors     | - Learns from mistakes<br>- Improves over time<br>- Works across sessions                        | - Needs strong evaluation<br>- May store “bad lessons”<br>- Requires vector DB/memory         | Long-running agents, autonomous researchers, dev copilots |
| **5. Tree-of-Thought (ToT)**          | ⭐⭐⭐⭐ _High_ — needs branching controller + scoring logic                | ⭐⭐⭐⭐ _Very High_ — explores multiple reasoning paths         | - Excellent reasoning depth<br>- Can recover from local errors<br>- Transparent exploration      | - Expensive (many model calls)<br>- Needs good heuristics<br>- Harder to debug                | Complex reasoning, theorem proving, strategy games        |
| **6. Graph-of-Thought / Multi-Agent** | ⭐⭐⭐⭐⭐ _Very High_ — requires orchestration, message passing, consensus | ⭐⭐⭐⭐⭐ _Highest_ — scalable, collaborative reasoning         | - Handles complex, decomposed tasks<br>- Enables specialization<br>- Parallel & fault-tolerant   | - Heavy infra & sync cost<br>- Hard to monitor/debug<br>- Requires governance & safety layers | Multi-agent systems, AI teams, enterprise orchestration   |

---

## 🧠 Summary Insights

| **Rank by Simplicity**                  | CoT → ReAct → Plan-Execute → Reflexion → ToT → Graph-of-Thought     |
| --------------------------------------- | ------------------------------------------------------------------- |
| **Rank by Result Quality**              | CoT < ReAct < Plan-Execute < Reflexion ≈ ToT < Graph-of-Thought     |
| **Best All-Around Trade-off**           | ⭐ **ReAct** — practical balance between complexity and performance |
| **Best for Long-Term Learning Agents**  | ⭐ **Reflexion**                                                    |
| **Best for Complex Reasoning / Search** | ⭐ **Tree-of-Thought**                                              |
| **Best for Large, Modular AI Systems**  | ⭐ **Graph-of-Thought / Multi-Agent**                               |

---

### Takeaway

> The more complex the planning algorithm, the more **infrastructure** and **compute cost** it requires — but also the higher its **reasoning depth, adaptability, and autonomy**.

> Most real-world production agents today sit around **ReAct + Reflexion hybrid territory**, while **ToT** and **Graph-of-Thought** architectures are emerging in advanced research and enterprise multi-agent systems.

## Planning Algorithms — Implementation Summary

| **Algorithm**                         | **Prompting** 🧾                                                            | **Controller / Logic** ⚙️                                         | **Data Structures** 🧮                        | **Persistence / Database** 🗄️                      | **Special Components** 🔧                                                     |
| ------------------------------------- | --------------------------------------------------------------------------- | ----------------------------------------------------------------- | --------------------------------------------- | -------------------------------------------------- | ----------------------------------------------------------------------------- |
| **1. Chain-of-Thought (CoT)**         | Few-shot or “think step-by-step” prompt; single model call                  | None (handled inline by LLM)                                      | None                                          | None (context only)                                | Optional verifier (secondary prompt or test)                                  |
| **2. ReAct (Reason + Act)**           | Structured prompt with `Thought:` / `Action:` / `Observation:` tags         | Iterative loop / FSM controller managing actions and observations | Scratchpad (list or JSON log of steps)        | In-memory only (ephemeral)                         | Tool registry (function map), stop criteria, error handling                   |
| **3. Plan-and-Execute**               | Two prompts: one for plan creation, one for subtask execution               | Planner + worker controller; task scheduler                       | Task queue / task graph                       | Optional artifact store (Redis, S3, DB)            | Replanner, task dispatcher, step success evaluator                            |
| **4. Reflexion**                      | Base reasoning prompt + reflection prompt (“analyze what went wrong/right”) | Two-phase pipeline: execution then reflection loop                | Run log, reflection record (text or JSON)     | Vector DB or doc store (FAISS, Weaviate, Pinecone) | Reflection retriever, decay/pruning scheduler                                 |
| **5. Tree-of-Thought (ToT)**          | Prompt to generate multiple reasoning branches (“produce N thoughts”)       | Search controller (BFS/DFS/beam search)                           | Tree or graph nodes, priority queue           | Optional KV store for node states                  | Scoring/evaluation function, heuristic selector, budget manager               |
| **6. Graph-of-Thought / Multi-Agent** | Role-specific prompts per agent; coordination prompt for routing            | Graph orchestrator (message passing, routing rules)               | Directed acyclic graph (DAG) or message queue | Shared vector DB + doc store (cross-agent context) | Router, consensus evaluator, message bus (Redis/Kafka/NATS), governance layer |

---

### 🧩 Key Takeaways

- **Prompting** is universal — every algorithm depends on specialized prompt design.
- **Controllers** grow in complexity: from none (CoT) → loop (ReAct) → planner (Plan-Execute) → orchestration (Graph-of-Thought).
- **Data structures** evolve from simple logs → queues → trees → graphs.
- **Persistence** appears first with Reflexion (memory) and expands with multi-agent systems.
- **Special components** (evaluators, routers, schedulers) are where systems gain autonomy and reliability.

## 1. Definitions

| Concept              | Description                                                                                                                                                                |
| -------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Stateless prompt** | Each model call is **independent** — it doesn’t include any conversational or historical context. The model “forgets” everything after the call.                           |
| **Stateful prompt**  | Each model call **includes accumulated context** (conversation history, reasoning trace, memory) so the model can reason with continuity.                                  |
| **External state**   | Information stored _outside_ the model (e.g., in a vector DB, cache, JSON, or task log). It’s **not automatically known to the model** unless re-injected into the prompt. |

---

## 2. Prompt Statefulness Across Planning Algorithms

| **Algorithm**                      | **Prompt Type**                                                        | **When Stateless**                                                                                         | **When Stateful**                                                         | **State Storage Mechanism (if external)**                                  |
| ---------------------------------- | ---------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------- | -------------------------------------------------------------------------- |
| **Chain-of-Thought (CoT)**         | _Stateless_ by default                                                 | Always — each reasoning happens within one model call                                                      | Only if you manually append previous reasoning (rare)                     | N/A (no external state)                                                    |
| **ReAct (Reason + Act)**           | Typically _stateful_ (history of Thought–Action–Observation in prompt) | Can be stateless **if** each iteration reloads only minimal previous context (e.g., last observation only) | Normally stateful, since reasoning depends on past steps                  | External JSON log or in-memory scratchpad (controller re-injects context)  |
| **Plan-and-Execute**               | Mixed — planner is _stateless_, executors are _semi-stateful_          | Stateless for planning phase (fresh prompt)                                                                | Stateful for executing subtasks (prompt includes plan context or results) | Task queue or DB storing subtask metadata                                  |
| **Reflexion**                      | Two stages: execution (can be stateful) + reflection (stateless)       | Stateless during reflection phase (analyzes one run at a time)                                             | Stateful during execution (accumulates reasoning)                         | Vector DB or document store for reflection memory                          |
| **Tree-of-Thought (ToT)**          | _Stateless per node_, state maintained externally                      | Always stateless — each node’s prompt regenerated from stored path                                         | N/A (reasoning path is reassembled externally)                            | Tree or graph structure (controller reconstructs context before each call) |
| **Graph-of-Thought / Multi-Agent** | _Mostly stateless per agent call_                                      | Usually stateless — each agent reads context from shared DB before generating                              | Can be stateful within agent if conversation history retained             | Shared vector DB / message bus (agents rehydrate context each call)        |

---

## 3. Rule of Thumb — When to Keep Prompts Stateful vs. Stateless

| **Goal**                              | **Prompt Strategy**                                | **Why**                                   |
| ------------------------------------- | -------------------------------------------------- | ----------------------------------------- |
| Short, isolated reasoning             | Stateless                                          | Saves tokens; no need for history         |
| Multi-step reasoning within same goal | Stateful                                           | Keeps continuity of logic                 |
| Long-term autonomous behavior         | Externally stateful (stateless prompt + memory DB) | Avoids context overflow; scalable         |
| Multi-agent collaboration             | Stateless (per agent) + shared external memory     | Enables parallel, distributed reasoning   |
| Debugging or deterministic replay     | Stateless                                          | Each step reproducible from logged inputs |

## 4. Architectural Trade-offs

| Aspect                        | **Stateful Prompt**        | **Stateless Prompt + External Memory** |
| ----------------------------- | -------------------------- | -------------------------------------- |
| Context continuity            | ✅ Built-in                | ⚙️ Rehydrated manually                 |
| Scalability                   | ❌ Limited by token window | ✅ Scales with external storage        |
| Cost (tokens per step)        | High                       | Lower                                  |
| Determinism / reproducibility | Lower (context drift)      | Higher (explicit context)              |
| Parallelization               | Harder                     | Easier (independent calls)             |
| Debugging                     | Simple traces              | More complex (multi-source context)    |

---

### 🧠 TL;DR

> - **Prompt = what the model sees right now.**
> - **State = what the system remembers overall.**
> - If you rebuild context every step from external memory → **stateless prompt, stateful system**.
> - If you append reasoning inline → **stateful prompt, short-lived state**.
