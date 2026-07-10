<div align="center">
  <img src="assets/logo.svg" width="96" alt="AI Trinity logo" />

  # AI Trinity — an operator's field manual

  **A great model is not enough. Here is what makes Claude actually reliable — and how to build it yourself.**

  [![License: CC BY 4.0](https://img.shields.io/badge/License-CC%20BY%204.0-lightgrey.svg)](LICENSE)
  [![Link Check](https://github.com/KeilerHirsch/ai-trinity/actions/workflows/link-check.yml/badge.svg)](https://github.com/KeilerHirsch/ai-trinity/actions/workflows/link-check.yml)

  [The Problem](manual/00-problem.md) · [The Thesis](manual/01-thesis.md) · [Pillar 1](manual/pillar-1-model/README.md) · [Pillar 2](manual/pillar-2-harness/README.md) · [Pillar 3](manual/pillar-3-memory/README.md) · [Field Notes](field-notes/README.md) · [Upstream](upstream/README.md)
</div>

---

## TL;DR

After months of heavy daily use, one conclusion held up: a brilliant model on its own is a **brilliant amnesiac**. The model is necessary but not sufficient. Three things have to be true at the same time before the quality-of-life jump actually happens:

| Pillar | Problem it solves | Reference project |
|--------|-------------------|-------------------|
| **[1. A model that is provably not dumb](manual/pillar-1-model/README.md)** | Error rate is the deciding factor, not benchmarks-on-paper. A high-hallucination model poisons everything downstream. | [bullshit-benchmark](https://github.com/petergpt/bullshit-benchmark) |
| **[2. A disciplined foundation / harness](manual/pillar-2-harness/README.md)** | Raw chat has no method. Skills, sub-agents, hooks, and review gates turn a chatbot into a tool. | [ECC](https://github.com/affaan-m/ECC) |
| **[3. A persistent brain](manual/pillar-3-memory/README.md)** | Session handoff and compaction are **architecturally lossy** — detail is silently dropped. Without external memory, every new session starts from zero. | [MemPalace](https://github.com/MemPalace/mempalace) |

Miss any one pillar and the system quietly degrades. This repo is a **field manual**: the problem, the thesis, a buildable path per pillar — and the receipts from actually running it.

## Battle-tested, not blogged

This is not theory written after a weekend of tinkering. The stack behind this manual runs daily against a 300k-entry memory store on Windows — and the failures it surfaced were diagnosed and **fixed upstream in the pillars themselves**: a merged data-loss fix, a 45s→1.3s MCP handshake fix, a silent-GPU-fallback fix, and more. See the [Upstream Ledger](upstream/README.md) for the receipts and the [Field Notes](field-notes/README.md) for the full worked cases.

## How to read this

1. **[The Problem](manual/00-problem.md)** — why a great model alone leaves you with an amnesiac genius.
2. **[The Thesis](manual/01-thesis.md)** — the three pillars and why all three are mandatory.
3. **The pillars** — one page each: claim, proof from the field, how to build it, how to operate it. [Model](manual/pillar-1-model/README.md) · [Harness](manual/pillar-2-harness/README.md) · [Memory](manual/pillar-3-memory/README.md). Build them in this order.
4. **[Field Conditions: Windows](manual/02-field-conditions-windows.md)** — the honest platform gaps nobody else documents.
5. **[Field Notes](field-notes/README.md)** — worked failure cases, and **[before/after proof](examples/README.md)** that the brain holds.
6. **[Upstream Ledger](upstream/README.md)** — the loop closed: what this setup sent back to its foundations.
7. **[FAQ](manual/03-faq.md)**

## Credits

This is one operator's lived system, standing on three independent open-source projects. Full credit to their authors — see [CREDITS](CREDITS.md). This repo contributes the **synthesis, the build path, the field notes — and bug fixes back upstream**.

## License

Documentation licensed under [CC BY 4.0](LICENSE). Maintained by **KeilerHirsch**. English-only by design.
