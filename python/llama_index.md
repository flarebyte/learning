### LlamaIndex: Quick Start in Python

LlamaIndex (formerly **GPT Index**) is a framework that helps you connect large language models (LLMs) to your own data — PDFs, databases, APIs, etc.

# 🦙 **LlamaIndex Python Cheatsheet**

## **Documents & Node Handling**

| Function / Class                               | Description                                                      | Example                                                                                                                  |
| ---------------------------------------------- | ---------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------ |
| `Document(text, metadata=None)`                | Basic text container with optional metadata.                     | `doc = Document(text="Hello world", metadata={"source": "file1"})`                                                       |
| `SimpleDirectoryReader(input_dir).load_data()` | Loads all supported files (txt, pdf, md, etc.) from a directory. | `docs = SimpleDirectoryReader("data").load_data()`                                                                       |
| `SentenceSplitter(chunk_size, chunk_overlap)`  | Splits documents into sentences or chunks for indexing.          | `python splitter = SentenceSplitter(chunk_size=512, chunk_overlap=20) nodes = splitter.get_nodes_from_documents(docs) `  |
| `TokenTextSplitter(chunk_size, chunk_overlap)` | Splits text by tokens, useful for embedding-based indexes.       | `python splitter = TokenTextSplitter(chunk_size=256, chunk_overlap=32) nodes = splitter.get_nodes_from_documents(docs) ` |

---

## **Indexes & Storage**

| Function / Class                             | Description                                 | Example                                                 |
| -------------------------------------------- | ------------------------------------------- | ------------------------------------------------------- |
| `VectorStoreIndex.from_documents(documents)` | Creates a vector index from your documents. | `index = VectorStoreIndex.from_documents(docs)`         |
| `VectorStoreIndex(nodes)`                    | Builds an index from pre-chunked nodes.     | `index = VectorStoreIndex(nodes)`                       |
| `index.as_query_engine()`                    | Turns an index into a query engine for Q&A. | `qe = index.as_query_engine()`                          |
| `index.insert(document)`                     | Adds a new document into an existing index. | `index.insert(new_doc)`                                 |
| `index.delete(doc_id)`                       | Removes a document or its vectors.          | `index.delete("doc_001")`                               |
| `index.save_to_disk(path)`                   | Saves index data for reuse later.           | `index.save_to_disk("index.json")`                      |
| `VectorStoreIndex.load_from_disk(path)`      | Loads a saved index.                        | `index = VectorStoreIndex.load_from_disk("index.json")` |

---

## **Querying & Responses**

| Function / Class                   | Description                                                    | Example                                                      |
| ---------------------------------- | -------------------------------------------------------------- | ------------------------------------------------------------ |
| `query_engine.query(query_str)`    | Executes a query over your data.                               | `resp = qe.query("Summarize the content")`                   |
| `query_engine.retrieve(query_str)` | Retrieves relevant source chunks without generating an answer. | `nodes = qe.retrieve("main topics")`                         |
| `Response.response`                | The plain text answer from the query.                          | `print(resp.response)`                                       |
| `Response.source_nodes`            | The list of source chunks used in the answer.                  | `python for src in resp.source_nodes: print(src.node.text) ` |
| `Response.get_formatted_sources()` | Formats and lists citations or source summaries.               | `print(resp.get_formatted_sources())`                        |

---

## **Prompting & Templates**

| Function / Class                   | Description                                      | Example                                                                           |
| ---------------------------------- | ------------------------------------------------ | --------------------------------------------------------------------------------- |
| `PromptTemplate(template_str)`     | Creates a reusable text prompt template.         | `tmpl = PromptTemplate("Context: {context}\nQuestion: {query}")`                  |
| `RichPromptTemplate(template_str)` | Advanced templating (e.g., logic, conditionals). | `tmpl = RichPromptTemplate("Answer given: {{ query }}")`                          |
| `ChatPromptTemplate(messages)`     | Defines system / user chat message templates.    | `tmpl = ChatPromptTemplate([("system", "You are helpful"), ("user", "{query}")])` |

---

## **LLM & Embedding Interfaces**

| Function / Class                                                            | Description                                           | Example                                                                                    |
| --------------------------------------------------------------------------- | ----------------------------------------------------- | ------------------------------------------------------------------------------------------ |
| `OpenAI(model, temperature=0)`                                              | Wrapper for OpenAI or compatible LLMs.                | `llm = OpenAI(model="gpt-4", temperature=0)`                                               |
| `LlamaAPI(api_key, **kwargs)`                                               | Interface for structured or function-based LLM usage. | `llm = LlamaAPI(api_key="key")`                                                            |
| `OpenAIPydanticProgram.from_defaults(output_cls, llm, prompt_template_str)` | Builds a structured output program using Pydantic.    | `python program = OpenAIPydanticProgram.from_defaults(MyOutputModel, llm, "Prompt text") ` |

---

## **Pipelines & Workflows**

| Function / Class                     | Description                                                | Example                                                                                                               |
| ------------------------------------ | ---------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------- |
| `IngestionPipeline(transformations)` | Chains steps (splitters, embedders, etc.) to process docs. | `python pipeline = IngestionPipeline(transformations=[TokenTextSplitter(...)]) nodes = pipeline.run(documents=docs) ` |
| `StorageContext()`                   | Manages persistence of embeddings, metadata, and indexes.  | `ctx = StorageContext.from_defaults()`                                                                                |
| `Settings`                           | Global settings for defaults like splitters or LLMs.       | `Settings.llm = OpenAI(model="gpt-4")`                                                                                |

---

## **Agents & Tools**

| Function / Class                 | Description                                                | Example                                                                                 |
| -------------------------------- | ---------------------------------------------------------- | --------------------------------------------------------------------------------------- |
| `FunctionTool.from_defaults(fn)` | Wraps a normal Python function as an LLM-callable tool.    | `python def greet(name): return f"Hi {name}" tool = FunctionTool.from_defaults(greet) ` |
| `QueryEngineTool(index, name)`   | Makes a query engine usable as an LLM agent tool.          | `tool = QueryEngineTool.from_defaults(index, name="knowledge_base")`                    |
| `ReActAgent(tools, llm)`         | Creates an agent that reasons and acts using tools.        | `agent = ReActAgent.from_tools([tool], llm=llm)`                                        |
| `AgentWorkflow()`                | Defines multi-step workflows for complex reasoning chains. | `workflow = AgentWorkflow(tools=[tool], llm=llm)`                                       |

---

## **Utilities**

| Function / Class                    | Description                                          | Example                                                     |
| ----------------------------------- | ---------------------------------------------------- | ----------------------------------------------------------- |
| `Settings.text_splitter = ...`      | Globally sets a default text splitter.               | `Settings.text_splitter = SentenceSplitter(chunk_size=400)` |
| `index.storage_context`             | Access the underlying storage or embedding DB.       | `ctx = index.storage_context`                               |
| `pipeline.add_transform(transform)` | Add a transformation step to a pipeline dynamically. | `pipeline.add_transform(TokenTextSplitter(...))`            |
