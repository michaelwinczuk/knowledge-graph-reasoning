# Knowledge Graph Reasoning

An [Agent Skill](https://github.com/anthropics/skills) for Claude that builds, queries, validates, and reasons over knowledge graphs. Turn unstructured information into structured, queryable, adversarially-validated knowledge.

Built by [Swarm Labs USA](https://swarmlabsusa.com) | [GitHub](https://github.com/michaelwinczuk)

## What It Does

This skill teaches Claude to work as a **knowledge engineer**:

- **Build** structured knowledge graphs from unstructured text, documents, or research
- **Query** graphs with traversal, path finding, hub detection, and neighbor search
- **Validate** new facts adversarially against existing knowledge (antonym, negation, consistency, and source checks)
- **Reason** over graphs using a 6-step deterministic pipeline: OBSERVE > CLASSIFY > HYPOTHESIZE > VERIFY > REFINE > DECOMPOSE
- **Maintain** graphs with health metrics, automatic splitting, and merge operations

## Why This Exists

LLMs hallucinate facts. Knowledge graphs don't.

This skill encodes a proven approach: **graph + composition + verification beats raw model reasoning**. Instead of asking an LLM to remember everything, structure knowledge as nodes and edges, validate every new fact against what's already known, and reason deterministically over the result.

The same principle is proven in the [Math Swarm](https://github.com/michaelwinczuk/math-swarm): replacing token prediction with deterministic computation achieves 100% accuracy where a 32B model alone achieves 93%. This skill applies the same principle to knowledge — structured retrieval instead of memorized guessing.

## Production Deployment

77 knowledge graphs deployed across 69 cluster domains. Measured results:

| Metric | Value |
|--------|-------|
| Knowledge graphs in production | 77 |
| Cluster domains covered | 69 (from finance to defense to healthcare) |
| Query cost | $0/query — all operations deterministic |
| False positive reduction | 60%+ with two-engine search (0.95 + 0.85 threshold) |
| Validation checks per fact | 4 (antonym, negation, consistency, source credibility) |
| Reasoning pipeline steps | 6 (OBSERVE > CLASSIFY > HYPOTHESIZE > VERIFY > REFINE > DECOMPOSE) |
| Edge types standardized | 10 typed relationships |
| Format | JSON-LD with `kg:` namespace |

### Domains Covered

The 77 graphs span: AI/ML, algorithms, ARC reasoning, architecture, blockchain, causal reasoning, chip fabrication, code quality, crypto, finance, game design, healthcare, math olympiad, mathematics, mechanical interpretability, networking, performance engineering, physics, quantum computing, robotics, security, and more.

## Quick Start

### Install in Claude Code

```bash
# From GitHub
/plugin install github:michaelwinczuk/knowledge-graph-reasoning
```

### Basic Usage

```
"Build a knowledge graph from these research notes about quantum computing..."

"I have a KG about our microservices architecture. Validate this new fact: 
 'Service A no longer depends on Service B since the March refactor.'"

"Find the path between 'user_signup' and 'revenue_event' in this graph."

"Run a health check on this knowledge graph."
```

## Key Concepts

### Adversarial Validation

Every new fact goes through a 4-check pipeline before being added to a graph:

1. **Antonym detection** — "X increases Y" vs existing "X decreases Y"
2. **Negation detection** — "X does not cause Y" vs existing "X causes Y"
3. **Consistency check** — Type mismatches, value range violations, temporal conflicts
4. **Source credibility** — Compares authority and recency of sources

Contradictions are **never silently resolved** — they're presented to the user with both sources.

### Two-Engine Search

Large graphs are searched in two passes:
- **Specific first** (0.95 threshold) — Exact/near-exact matches
- **General fallback** (0.85 threshold) — Fuzzy matching if specific returns nothing

Prevents irrelevant results from drowning out precise matches.

### Deterministic Reasoning

The 6-step reasoning pipeline (OBSERVE > CLASSIFY > HYPOTHESIZE > VERIFY > REFINE > DECOMPOSE) produces reproducible analysis over graph data. Confidence scores propagate through inference chains with automatic decay.

### 10 Standardized Edge Types

| Type | Meaning |
|------|---------|
| `causes` | A leads to B |
| `requires` | A needs B |
| `contradicts` | A conflicts with B |
| `supports` | A provides evidence for B |
| `contains` | A includes B |
| `precedes` | A comes before B |
| `similar_to` | A resembles B |
| `derived_from` | A was created from B |
| `influences` | A affects B indirectly |
| `instance_of` | A is an example of B |

## Project Structure

```
knowledge-graph-reasoning/
├── SKILL.md                    # Core skill (loaded by Claude)
├── README.md                   # This file
├── LICENSE                     # Apache 2.0
├── scripts/
│   ├── validate.py             # Adversarial fact validation
│   └── traverse.py             # Graph traversal and query engine
├── references/
│   ├── schemas.md              # Complete JSON-LD schemas for nodes, edges, graphs
│   └── examples.md             # Worked examples with real graphs
└── evals/
    └── evals.json              # 5 test cases for skill evaluation
```

## Swarm Labs Ecosystem

| Project | What It Does | Status |
|---------|-------------|--------|
| [Math Swarm](https://github.com/michaelwinczuk/math-swarm) | Zero-hallucination computation. 1,079 tests, 100%, 12 categories, 15 clinical formulas. | 1,079 tests passing |
| [Swarm Orchestrator](https://github.com/michaelwinczuk/swarm-orchestrator) | Multi-agent design patterns, chalkboard protocol, deterministic-first cascade. | 5+ swarms validated |
| [PRISM](https://github.com/michaelwinczuk/prism) | Reliability primitives — VotingMesh, Sentinel, checkpoint/replay. | 95 tests passing |
| [Bastion](https://github.com/michaelwinczuk/bastion) | Safety kernel — consensus, verification, SHA-256 audit trails. | Rust + Tokio |
| [Swarm Labs USA](https://swarmlabsusa.com) | Autonomous AI systems for government. | Active |

## License

Apache 2.0 — See [LICENSE](LICENSE) for details.

---

Built by [Michael Winczuk](https://github.com/michaelwinczuk) at [Swarm Labs USA](https://swarmlabsusa.com)
