🌐 **English** · [Deutsch](de/02-thesis.md)

[← Back to README](../README.md) · [Problem](01-problem.md) · **Thesis** · [Build Guide](03-build-guide.md) · [Windows Reality](04-windows-reality.md) · [FAQ](05-faq.md) · [In the Wild](06-in-the-wild.md)

---

# The Thesis: three pillars, all mandatory

A reliable AI working setup needs three things to be true **at the same time**. Each pillar fails differently when missing, and no pillar compensates for another. This is the whole claim.

```
        model quality          (is the answer trustworthy?)
            /\
           /  \
          /    \
 foundation —— persistent brain
 (is there a    (does it survive
  method?)       the next session?)
```

## Pillar 1 — A model that is provably not dumb

**Claim:** error rate is the deciding property of a working model, more than raw capability or paper benchmarks.

A model you build a system around must be screened on how often it is *wrong* — especially how often it is confidently wrong on the things you can't easily check. If the model invents facts, everything downstream (your memory, your research, your decisions) inherits that error and compounds it.

Practically this means: measure hallucination/error rate objectively, and refuse to route consequential work through models that fail the screen — no matter how impressive they look in a demo.

→ Reference: [bullshit-benchmark](https://github.com/petergpt/bullshit-benchmark) — an objective screen for model nonsense.

## Pillar 2 — A disciplined foundation (harness)

**Claim:** a chatbot becomes a tool only when there is enforced method around it.

Raw chat improvises. A harness gives you reusable skills, specialized sub-agents, review gates, and hooks that fire automatically — so the *process* is reliable, not just the occasional good answer. It is the difference between "ask and hope" and "a procedure that runs the same way every time, with verification built in."

→ Reference: [ECC](https://github.com/affaan-m/ECC) — skills, agents, hooks, slash commands, and review gates layered on top of the agent runtime.

## Pillar 3 — A persistent brain

**Claim:** since handoff/compaction is architecturally lossy ([see the problem](01-problem.md)), the only fix is memory that lives *outside* the conversation and is reloaded into every session.

A persistent store — written to automatically as you work, and read back at the start of each session — turns the amnesiac into something continuous. Yesterday's hard-won understanding is there today, without you re-explaining it. The store must be **ruthless about quality** (see Pillar 1): a memory that ingests noise is worse than no memory, because it launders bad data into "remembered fact."

→ Reference: [MemPalace](https://github.com/MemPalace/mempalace) — a local vector-store memory, auto-saved via runtime hooks and reloaded at session start.

## Why all three, and in this order

| You have | You don't have | Result |
|----------|----------------|--------|
| Model | Method + Memory | Brilliant amnesiac improvising daily |
| Model + Method | Memory | Fast, repeatable — but still starts from zero every session |
| Model + Memory | Method | Persistent, but undisciplined and unverifiable |
| Method + Memory | Trustworthy model | A well-organized, permanent archive of confident nonsense |
| **All three** | — | A system that is reliable, continuous, and gets *better* over time |

The pillars are not a menu. They are a set of simultaneous conditions. The payoff — the thing that makes it feel like "there was never a pause" — only appears when all three hold.

Next: **[how to build it](03-build-guide.md)**.
