<div align="center">
  <img src="assets/logo.svg" width="96" alt="AI Trinity logo" />

  # AI Trinity

  **A great model is not enough. Here is what makes Claude actually reliable — and how to build it yourself.**

  [![License: CC BY 4.0](https://img.shields.io/badge/License-CC%20BY%204.0-lightgrey.svg)](LICENSE)
  [![Link Check](https://github.com/KeilerHirsch/ai-trinity/actions/workflows/link-check.yml/badge.svg)](https://github.com/KeilerHirsch/ai-trinity/actions/workflows/link-check.yml)
  [![EN/DE](https://img.shields.io/badge/docs-EN%20%7C%20DE-blue.svg)](docs/de/README.md)

  🌐 **English** · [Deutsch](docs/de/README.md)

  [Problem](docs/01-problem.md) · [Thesis](docs/02-thesis.md) · [Build Guide](docs/03-build-guide.md) · [Windows Reality](docs/04-windows-reality.md) · [FAQ](docs/05-faq.md) · [In the Wild](docs/06-in-the-wild.md)
</div>

---

## TL;DR

After months of heavy daily use, one conclusion held up: a brilliant model on its own is a **brilliant amnesiac**. The model is necessary but not sufficient. Three things have to be true at the same time before the quality-of-life jump actually happens:

| Pillar | Problem it solves | Reference project |
|--------|-------------------|-------------------|
| **1. A model that is provably not dumb** | Error rate is the deciding factor, not benchmarks-on-paper. A high-hallucination model poisons everything downstream. | [bullshit-benchmark](https://github.com/petergpt/bullshit-benchmark) |
| **2. A disciplined foundation / harness** | Raw chat has no method. Skills, sub-agents, hooks, and review gates turn a chatbot into a tool. | [ECC](https://github.com/affaan-m/ECC) |
| **3. A persistent brain** | Session handoff and compaction are **architecturally lossy** — detail is silently dropped. Without external memory, every new session starts from zero. | [MemPalace](https://github.com/MemPalace/mempalace) |

Miss any one pillar and the system quietly degrades. This repo is the **problem statement + a buildable solution** — not a product to install, but a map you can reproduce.

## Why this exists

There is a lot of confident noise about "getting the most out of Claude." Most of it is vibes. This is the opposite: a concrete, reproducible account of what failed, why, and what fixed it — with **before/after proof** in [`examples/`](examples/), not claims.

## Start here

1. **[The Problem](docs/01-problem.md)** — why a great model alone leaves you with an amnesiac genius.
2. **[The Thesis](docs/02-thesis.md)** — the three pillars and why all three are mandatory.
3. **[The Build Guide](docs/03-build-guide.md)** — step by step, pointing at the three canonical projects.
4. **[Windows Reality](docs/04-windows-reality.md)** — the honest gaps nobody else documents.
5. **[In the Wild](docs/06-in-the-wild.md)** — concrete tools built directly on this thesis, starting with the author's own.

## Credits

This is one operator's lived system, standing on three independent open-source projects. Full credit to their authors — see [CREDITS](CREDITS.md). This repo only contributes the **synthesis, the build path, and the field notes**.

## License

Documentation licensed under [CC BY 4.0](LICENSE). Maintained by **KeilerHirsch**.
