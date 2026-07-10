[← Manual](../../README.md) · [Problem](../00-problem.md) · [Thesis](../01-thesis.md) · [Pillar 1: Model](../pillar-1-model/README.md) · **Pillar 2: Harness** · [Pillar 3: Memory](../pillar-3-memory/README.md)

---

# Pillar 2 — A disciplined foundation (harness)

**Claim:** a chatbot becomes a tool only when there is enforced method around it.

Raw chat improvises. A harness gives you reusable skills, specialized sub-agents, review gates, and hooks that fire automatically — so the *process* is reliable, not just the occasional good answer. It is the difference between "ask and hope" and "a procedure that runs the same way every time, with verification built in".

## Proof from the field

- **Review gates catch what confidence misses.** The day the preload-DLLs fix went upstream ([PR ledger](../../upstream/README.md)), a mandatory pre-push code-review pass flagged three real defects in code that "looked done": a thread race on a one-shot flag, a failure path logged at debug level where operators would never see it, and existing tests that would have triggered real DLL loading on CI. All three were fixed before the PR went out. That is the harness working: the method found what the author missed.
- **Guard hooks stop the bad day.** Hooks that block secret-file reads and gate destructive commands do not matter until the day they do. They have fired in this setup — on real commands, written in good faith, that would have been wrong.

## Build it

1. Install [ECC](https://github.com/affaan-m/ECC) as a plugin/marketplace for your runtime (follow its README for your platform).
2. Confirm the layer is live: its skills, agents, and slash-commands appear in your session and actually run.
3. Adopt the pieces that match *your* work — review gates, build/test skills, security guards — and turn off what you do not use. A lean, intentional set beats the full firehose.
4. Add guardrail hooks for the dangerous defaults: block secret-file reads, deny destructive commands, keep all hook paths workspace-relative so a moved folder cannot break them.

## Operate it

- **Review before push, every time** — not when it feels risky. The whole point of a gate is that it does not depend on your judgment of when it is needed.
- Let hooks do the remembering: datetime injection, session-start context, auto-save on stop. Anything you would have to remember to do manually will eventually not get done.
- Treat hook and skill configs as code: version them, and when one misbehaves, debug it with the same rigor as the product (see the [timeout-units lesson](../02-field-conditions-windows.md) for why).

**Verify:** a slash-command runs end to end; a guard hook actually blocks something it should.

→ Next: [Pillar 3 — the persistent brain](../pillar-3-memory/README.md)
