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
