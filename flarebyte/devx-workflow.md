# Overview of the Baldrick DevX Workflow

This workflow defines a **complete development lifecycle** for library and application projects within the company.
It is intentionally **language-agnostic** — similar versions exist for TypeScript, Python, Go, Dart, and other ecosystems.
The workflow is designed to promote **consistency, automation, and quality** across all projects, while remaining **modular and extensible**.


## Core Philosophy

> “A developer should spend their time thinking about problems, not customising process.”

The workflow captures **repeatable DevOps and DevX practices** as declarative tasks written in YAML.
Each task is well-motivated, auditable, and can be executed individually or as part of a larger sequence.

It aims to:

* Reduce onboarding friction.
* Ensure predictable quality gates.
* Encourage convention over configuration.
* Facilitate both human and AI collaboration.

---

## Structure at a Glance

The workflow is divided into **high-level areas** called *workflows* — each containing *tasks*.
Each task represents a meaningful step in the software lifecycle (e.g., testing, linting, building, documenting).

### Top-Level Workflows

| Workflow              | Purpose                                                        |
| --------------------- | -------------------------------------------------------------- |
| **test**              | Verify quality through unit, coverage, and acceptance testing. |
| **transpile / build** | Compile or transpile the source code into distributable form.  |
| **deps**              | Manage and upgrade project dependencies.                       |
| **doc**               | Automatically generate developer and API documentation.        |
| **github**            | Standardize and manage GitHub project configuration.           |
| **lint**              | Perform static code analysis and automatic formatting.         |
| **md**                | Check and fix Markdown documentation consistency.              |
| **release**           | Prepare, validate, and publish releases to package registries. |
| **scaffold**          | Bootstrap or normalize project structure and metadata.         |

---

## Key Workflow Highlights

### **Test**

Ensures the code behaves correctly and remains reliable as it evolves.

* **Unit tests** validate logic in isolation.
* **Acceptance tests** (via *pest*) confirm expected CLI or API behavior.
* **Coverage analysis** (e.g., via c8 or equivalent) tracks how much code is tested, helping prevent regressions.

**Motivation:**

> “Quality is not an afterthought — it’s built in.”

---

### **Transpile / Build**

Translates source code into its runnable or distributable form.
For compiled languages, this means building binaries; for scripting languages, preparing artifacts.

**Motivation:**

> “The build should be clean, reproducible, and environment-independent.”

---

### **Dependencies**

Keeps the project’s dependencies secure and up to date.

* Regular upgrades using package managers (yarn, pip, go mod, etc.).
* Automated prompts to review changes interactively.

**Motivation:**

> “Stay current to stay secure.”

---

### **Documentation**

Generates markdown or HTML documentation automatically from the source code and metadata.

* Converts comments or docstrings into API docs.
* Produces supporting materials like dependency lists or CSV summaries.
* Cleans and formats all generated markdown for consistency.

**Motivation:**

> “Documentation is a first-class deliverable — not an optional extra.”

---

### **Linting and Formatting**

Enforces style and static checks (via tools like Biome, Ruff, or Golangci-lint).

* Identifies issues early.
* Can automatically fix and format code.

**Motivation:**

> “A consistent codebase is easier to read, review, and refactor.”

---

### **Markdown Management**

Applies consistent editorial standards across all documentation files, including GitHub templates.

**Motivation:**

> “Readable, professional documentation improves collaboration and trust.”

---

### **Release**

Automates checks and publishing before a release.

Steps include:

* Lint and test validation.
* Dependency audits and security checks.
* Optional integration (acceptance) tests.
* Versioning and changelog validation.
* Report generation for pull requests and CI pipelines.
* Optional publishing to registries and GitHub releases.

**Motivation:**

> “Every release should be predictable, transparent, and reversible.”

---

### **Scaffold / Normalize**

Creates or refreshes standard project files (configuration, templates, metadata).

* Generates GitHub workflows and templates.
* Normalizes editor settings, linters, and documentation.
* Reconstructs `package.json` or equivalent metadata to match organizational standards.
* Can use either **declarative templates** or **custom scripts** for unique cases.

**Motivation:**

> “Consistency across projects reduces cognitive load and improves discoverability.”

---

## Ever-Evolving Workflow

This DevX framework is **continuously improved** — each iteration of the YAML reflects collective learning across projects and languages.
As tooling, languages, and practices evolve, so does this workflow.
Developers are encouraged to:

* Contribute improvements.
* Reuse patterns in new ecosystems.
* Treat automation as a living artifact, not a frozen template.

> “The workflow evolves as our engineering culture evolves.”

---

## Summary for Onboarding

If you’re a new **developer or AI assistant**, remember:

* Each workflow area defines a **standard responsibility** (test, lint, build, doc, etc.).
* Each task has a **clear motivation** — why it exists, not just what it does.
* You can **run tasks independently** or **combine them** for full lifecycle automation.
* The YAML structure is both **human-readable** and **machine-executable** — ideal for hybrid (human + AI) development.

