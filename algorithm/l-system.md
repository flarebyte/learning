# L-System

## What is an L-System (Lindenmayer System)?

An **L-system** is a rule-based system for generating sequences of symbols (usually strings) that simulate growth — originally for modeling plant development. Think of it as a **rewrite engine**: you start with an initial string (`axiom`) and apply rules (`productions`) that replace symbols with other strings, step by step.

### 🧠 Key Concepts:

- **Axiom**: Starting string. Like `"A"`.
- **Rules**: Dictate how each character is transformed. For example, `"A" → "AB"`, `"B" → "A"`.
- **Iterations**: Each step rewrites the entire string based on the rules.

```python
axiom = "A"
rules = {
    "A": "AB",
    "B": "A"
}
```

Each iteration rewrites the string according to the rules. After a few steps, the string grows into something like: `"ABAABABA..."`

## What is an Extended L-System?

**Extended L-systems** take the original concept and **add more structure or logic**, making them more flexible and programmable. These extensions often include:

1. **Parameterized L-systems** – where symbols can have parameters (like function calls).
2. **Context-sensitive L-systems** – rules can depend on neighboring symbols.
3. **Stochastic L-systems** – introduce randomness to rules.
4. **Functional or procedural L-systems** – symbols can represent actions, like turtle graphics (drawing), or even contain Python-like logic.

```python
# A symbol with parameters
rules = {
    "F(x)": lambda x: f"F({x+1})G({x-1})" if x > 0 else ""
}
```

## Comparison Summary

| Feature                   | **L-System**                         | **Extended L-System**                                      |
| ------------------------- | ------------------------------------ | ---------------------------------------------------------- |
| **Simplicity**            | Very simple, rule-based              | More complex, supports parameters, logic, context          |
| **Data Structure**        | Basic strings of characters          | Symbols with data/parameters (like tokens or objects)      |
| **Customization**         | Limited to fixed rules               | Highly customizable (conditions, randomness, etc.)         |
| **Expressiveness**        | Good for regular, recursive patterns | Capable of complex, naturalistic or abstract growth models |
| **Performance**           | Fast and lightweight                 | Slower due to complexity                                   |
| **Implementation Effort** | Minimal (a few lines of code)        | Moderate to high (need parsing, evaluation, etc.)          |
| **Use Cases**             | Fractals, simple plant shapes        | Realistic plants, procedural generation, simulations       |

## Use Cases

| **Domain**             | **Example**                                             |
| ---------------------- | ------------------------------------------------------- |
| **Graphics**           | Generating procedural trees, fractals, and coral shapes |
| **Games**              | World generation (plants, terrain details)              |
| **Biology/Simulation** | Modeling plant growth, cellular development             |
| **Art & Design**       | Creating recursive or organic patterns                  |
| **Education**          | Teaching recursion, algorithms, and emergence           |

## L-System with Graphs (Graph-Based L-System)

### Concept:

Instead of representing the system as a string of characters, we use a **graph** where:

- **Nodes** represent entities (e.g. branches, leaves, junctions).
- **Edges** represent relationships or connections between entities.
- **Rules** apply to **subgraphs or nodes** and generate new nodes or modify structure.

This is closer to how real-life growth works (e.g., a plant node splits into branches, not into characters).

## Key Components

| Component       | Description                                                      | Example (Python-ish)                            |
| --------------- | ---------------------------------------------------------------- | ----------------------------------------------- |
| `graph`         | The current structure – usually a `dict` or graph object         | `graph[node] = [child1, child2]`                |
| `node`          | A unit with attributes (like type, age, angle, etc.)             | `{"type": "branch", "length": 2}`               |
| `rule(node)`    | A function that returns changes (e.g., new nodes or connections) | `if node["type"] == "tip": grow new branches()` |
| `apply_rules()` | Walk through nodes and apply growth rules                        | Loop over graph, apply rule per node            |

## Example Structure (in pseudo-Python)

```python
graph = {
    0: {"type": "root", "children": [1, 2]},
    1: {"type": "tip", "children": []},
    2: {"type": "tip", "children": []}
}

def growth_rule(node_id, graph):
    node = graph[node_id]
    if node["type"] == "tip":
        new_id = max(graph) + 1
        graph[new_id] = {"type": "tip", "children": []}
        node["children"].append(new_id)
        node["type"] = "branch"
```

Each iteration modifies the graph by **adding nodes** and **updating connections** based on the rules. You can visualize this as a growing tree with nodes branching out.

## Graph L-System Summary

| Feature             | Description                                                             |
| ------------------- | ----------------------------------------------------------------------- |
| **Structure Type**  | Graph (nodes + edges) instead of linear strings                         |
| **Data-rich**       | Nodes can store attributes (age, length, direction, etc.)               |
| **Flexible Growth** | Growth can occur from multiple points, not just left-to-right           |
| **Ideal For**       | Trees, biological models, structural modeling, 3D object generation     |
| **Tools**           | Python + `networkx`, or game engines with graph support (Unity, Unreal) |

## When to Use Graph-Based L-systems

| Use Case                    | Why Graph L-System Works Well                                 |
| --------------------------- | ------------------------------------------------------------- |
| **Botanical models**        | Branching structures with rich geometry                       |
| **3D modeling**             | Handles space-aware node placement and spatial relationships  |
| **Procedural architecture** | Supports rule-based generation of rooms, hallways, structures |
| **Biological simulations**  | Cells, organs, or ecosystems with spatial dependency          |
