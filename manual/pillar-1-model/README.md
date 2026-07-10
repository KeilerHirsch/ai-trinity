[← Manual](../../README.md) · [Problem](../00-problem.md) · [Thesis](../01-thesis.md) · **Pillar 1: Model** · [Pillar 2: Harness](../pillar-2-harness/README.md) · [Pillar 3: Memory](../pillar-3-memory/README.md)

---

# Pillar 1 — A model that is provably not dumb

**Claim:** error rate is the deciding property of a working model — more than raw capability, more than paper benchmarks.

A model you build a system around must be screened on how often it is *wrong*, especially how often it is confidently wrong on things you cannot easily check. If the model invents facts, everything downstream — your memory, your research, your decisions — inherits the error and compounds it.

## Proof from the field

- **The archive-of-nonsense failure is real.** An earlier iteration of this setup routed memory-writing work through a weaker model. The persistent store filled with plausible-sounding noise, and later sessions reasoned over it as if it were fact. The store had to be rebuilt and the model dropped. A memory that ingests noise is worse than no memory — it launders bad data into "remembered fact".
- **Validators need the same screen.** Using a high-hallucination model to *check* another model's work fails the same way, just less visibly: a validator that invents its own findings poisons your review process instead of your memory. Same screen, same bar.

## Build it

1. Read the screen: [bullshit-benchmark](https://github.com/petergpt/bullshit-benchmark). Understand *what* it measures (confident wrongness), not just the score.
2. Decide your workhorse model and write down *why* it passes your bar.
3. Make it a rule: never route consequential or memory-writing work through a model that fails the screen. Drop "multi-model" setups where a weak model gets a vote — its errors cost you later, somewhere else, harder to trace.

## Operate it

- Keep the workhorse **stable**. Chasing the model-of-the-week resets your calibration for how it fails; you want to know its failure modes cold.
- Re-screen after major model updates. Behavior shifts; your bar does not.
- When something downstream looks poisoned (memory entries that do not match reality, review findings that cite nothing), suspect a model-quality breach first and trace what wrote it.

**Verify:** you can state, in one sentence, which model you trust for write-to-memory work and why.

→ Next: [Pillar 2 — the harness](../pillar-2-harness/README.md)
