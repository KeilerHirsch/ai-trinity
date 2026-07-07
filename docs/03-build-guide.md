🌐 **English** · [Deutsch](de/03-build-guide.md)

[← Back to README](../README.md) · [Problem](01-problem.md) · [Thesis](02-thesis.md) · **Build Guide** · [Windows Reality](04-windows-reality.md) · [FAQ](05-faq.md) · [In the Wild](06-in-the-wild.md)

---

# The Build Guide

This is the reproducible path. It does **not** hand you a config to paste — your setup should be yours. It tells you what to assemble, in what order, and how to verify each pillar actually holds.

> Order matters. Build Pillar 1 first (so you don't memorize noise), then Pillar 2 (so you have method), then Pillar 3 (so it persists). Verify each before moving on.

## Prerequisites

- An agent runtime you drive daily (e.g. Claude Code in your editor or terminal).
- A capable model as your workhorse. Pick the most capable available to you and keep it stable.
- Comfort with your shell, `git`, and editing JSON config. That's the floor.

## Pillar 1 — Screen the model (don't skip this)

**Goal:** know your model's error rate before you trust it with memory.

1. Read the screen: [bullshit-benchmark](https://github.com/petergpt/bullshit-benchmark). Understand *what* it measures (confident wrongness), not just the score.
2. Decide your workhorse model and write down *why* it passes your bar.
3. Make it a rule: never route consequential or memory-writing work through a model that fails the screen. Drop "multi-model" setups where a weak model gets a vote — its errors cost you later (see [Problem §3](01-problem.md)).

**Verify:** you can state, in one sentence, which model you trust for write-to-memory work and why.

## Pillar 2 — Install the foundation (harness)

**Goal:** replace improvisation with enforced method.

1. Install [ECC](https://github.com/affaan-m/ECC) as a plugin/marketplace for your runtime (follow its README for your platform).
2. Confirm the layer is live: its skills, agents, and slash-commands appear in your session and actually run.
3. Adopt the pieces that match *your* work — review gates, build/test skills, security guards — and turn off what you don't use. A lean, intentional set beats the full firehose.
4. Add guardrail hooks for the dangerous defaults: block secret-file reads, deny destructive commands, keep all hook paths workspace-relative so a moved folder can't break them.

**Verify:** a slash-command runs end to end; a guard hook actually blocks something it should.

## Pillar 3 — Stand up the persistent brain

**Goal:** memory that writes itself as you work and reloads at session start.

1. Install [MemPalace](https://github.com/MemPalace/mempalace) (or an equivalent local memory store) and connect it to your runtime as an MCP server.
2. **Wire auto-save through hooks** — this is the part people skip and then wonder why memory is empty:
   - a **Stop** hook and a **pre-compaction** hook that write the session's notes/state *before* context is lost;
   - a **session-start** hook that injects the recovered context back in.
3. **Keep it clean** (Pillar 1 applies to your own store): filter secrets/junk before ingest, dedup so repeated ingests don't bloat, and never let a noisy model write unreviewed "facts" into it.
4. Keep a tiny, human-readable index of what's stored, so both you and the assistant can see the memory's shape at a glance.

**Verify:** end a session, start a new one, and ask about something only the previous session knew. If it answers from memory without you re-explaining — the brain holds.

## Putting it together

A working day now looks like: session starts → memory + instincts load automatically → you work with method (harness) → a trustworthy model reasons over correct context → on exit, the session is saved without you asking. Tomorrow continues where today ended.

That continuity is the entire point. When it works, it feels like **there was never a pause**.

→ Before you celebrate on Windows, read [Windows Reality](04-windows-reality.md): some of the auto-save/learning machinery has real platform gaps, with workarounds.
