# slow-slow-quick-quick

A collection of AI assistant skills built around intentional friction. It slows down interactions to deepen user's understanding, form muscle memory, and produce work that reflects the user's genuine thinking rather than AI-generated output.

The name reflects the deliberate practice philosophy: **go slow now to go fast later.**

---

## Skills

| Skill | Trigger | What it does |
|-------|---------|--------------|
| `slow-vibe-coding` | Implementing a feature, writing a function, solving a coding problem | Refuses to write implementation code. Brainstorms patterns, presents at least two concrete approaches with trade-offs, provides comment-only skeletons, and guides you to write it yourself. |
| `slow-vibe-sw-architect` | System design, architecture decisions, technical choices | Refuses to give an immediate answer. Asks trade-off questions and failure scenarios until you arrive at a decision you own. Produces an ADR. |
| `slow-dev-research` | Technology selection, stack comparisons, engineering trade-off questions | Refuses to give a recommendation upfront. Surfaces research and real-world cases with sources, asks requirements questions one at a time, and guides you to a conclusion you can defend. Produces a research document. |


### Tier system (`slow-vibe-coding`)

- **Tier 1 (default):** You write all code. AI provides skeleton + comments + hints only.
- **Tier 2 (guided):** You make explicit decisions at each step. AI may write individual sub-functions after you explain your intent. You must be able to explain every line.

Tier 2 is only entered if explicitly requested.

---

## Project Structure

```
slow-slow-quick-quick/
├── skills/
│   ├── slow-vibe-coding/          # Each skill is a directory
│   ├── slow-vibe-sw-architect/
│   ├── slow-dev-research/
│   └── scribe/     # TODO
├── scripts/
├── tests/
└── README.md
```

---

## Installation
// TODO:

---

## Usage (WIP)

Each skill can be invoked explicitly:

```
/slow-vibe-coding
/slow-vibe-sw-architect
```

Or the AI activates them automatically when context matches (e.g., you ask to implement a feature → `slow-vibe-coding` activates).

---

## How to Combine Skills

The skills are designed to chain. Each one ends by prompting a natural transition to the next.

### The Full Pipeline

```
slow-dev-research → slow-vibe-sw-architect → slow-vibe-coding
```

**Phase 1 — Research (`slow-dev-research`)**
- **Input:** A technical question or source material (benchmarks, docs, case studies)
- **Output:** Research document with trade-off map and your conclusion
- **Handoff:** AI prompts transition to `slow-vibe-sw-architect` when an architectural decision emerges

**Phase 2 — Architecture (`slow-vibe-sw-architect`)**
- **Input:** Research document from Phase 1
- **Output:** Completed ADR
- **Handoff:** AI explicitly prompts you to begin implementation with `slow-vibe-coding`

**Phase 3 — Implementation (`slow-vibe-coding`)**
- **Input:** ADR from Phase 2
- **Output:** Working, user-owned code

**Artifact-driven context:** Instead of hidden context sharing, the pipeline relies on explicit output artifacts. The finalized artifact of one step becomes the mandatory input context for the next. This forces you to review, own, and approve the intermediate context before the AI proceeds.


### Partial Chains

- **Research only:** Use `slow-dev-research` when you need to compare options before committing to an architecture.
- **Architecture only:** Start with `slow-vibe-sw-architect` if you already have the research.
- **Coding only:** Start with `slow-vibe-coding` if you have an ADR — pass it in as context.

---

## Philosophy

These skills exist because AI-generated output you don't understand is a liability, not an asset.

- An architecture you didn't reason through will be operated wrong.
- Code you didn't write will fail in ways you can't explain.
- A summary you didn't interrogate will anchor your thinking to someone else's reading.

The slow skills make AI assistance into a collaboration where *you* are the author — the AI is the guide.

---

## Non-Goals

- No enforcement mechanism — compliance depends on the AI following instructions
- No persistent memory across sessions — Scribe outputs are per-session files saved manually by the user
- No UI — purely text-based skill invocation
- No automatic file writes — all output is presented as copy-pasteable blocks
