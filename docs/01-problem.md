🌐 **English** · [Deutsch](de/01-problem.md)

[← Back to README](../README.md) · **Problem** · [Thesis](02-thesis.md) · [Build Guide](03-build-guide.md) · [Windows Reality](04-windows-reality.md) · [FAQ](05-faq.md) · [In the Wild](06-in-the-wild.md)

---

# The Problem: a brilliant amnesiac

The model is not the bottleneck. The model is excellent. The bottleneck is everything *around* the model — and if you only fix the model, you are left with a brilliant amnesiac who is occasionally confidently wrong.

Three failure modes show up, in order of how much pain they cause over time.

## 1. Lossy handoff — the amnesia

Long conversations get **compacted**: the runtime summarizes earlier turns to fit the context window. Compaction is, by design, **lossy** — it keeps the gist and drops detail. So does starting a "fresh" session.

The damage is not abstract. Picture a session where the assistant builds a genuinely sharp, well-evidenced understanding of a complex topic you care about — your situation, your project, your constraints. It reasons with full context, connects things you didn't spell out, gets it *right*.

Then the session ends.

The next session knows none of it. Not the conclusions, not the evidence, not the nuance. You re-explain, or worse, you don't — and the assistant quietly operates with less than it had yesterday. The single most useful thing the system ever produced is the first thing it forgets. **Continuity is the product, and continuity is exactly what a stateless chat cannot give you.**

Handoff notes and "summarize for next time" tricks don't solve this. They are lossy on top of lossy: a summary of a summary. The detail that made yesterday's session good is precisely the detail a handoff drops.

## 2. No method — raw chat has no discipline

A chat window is a blank prompt every time. There is no enforced workflow, no review step, no "did you actually verify that," no reusable procedure for the things you do repeatedly. You get whatever the model improvises that day.

For one-off questions that's fine. For real, repeated work — building software, running research, handling anything with consequences — improvisation is not a method. You need skills, sub-agents, gates, and hooks that make the *process* reliable, not just the individual answer.

## 3. Unreliable models poison everything downstream

Not all models are equally honest. A model with a high hallucination rate doesn't just give you a wrong answer once — if you let it write to your notes, your memory, your research, it **poisons the well**. Every later session then reasons over corrupted inputs and compounds the error.

This is the trap behind naïve "multi-model" setups: routing work through a model that confidently invents facts means the cost of its errors shows up later, somewhere else, harder to trace. Error rate is not a detail. It is the foundation everything else sits on.

## Why fixing one is not enough

- A great model with no memory → brilliant amnesiac (failure 1).
- A great model with memory but no method → fast, persistent chaos (failure 2).
- Memory + method but a noisy model → a well-organized, persistent archive of confident nonsense (failure 3).

The failures interlock. That is why the solution is not one tool but **three pillars that must all hold at once** — the subject of [the thesis](02-thesis.md).
