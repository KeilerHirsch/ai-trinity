[← Manual](../../README.md) · [Problem](../00-problem.md) · [Thesis](../01-thesis.md) · [Pillar 1: Model](../pillar-1-model/README.md) · [Pillar 2: Harness](../pillar-2-harness/README.md) · **Pillar 3: Memory**

---

# Pillar 3 — A persistent brain

**Claim:** since handoff/compaction is architecturally lossy ([see the problem](../00-problem.md)), the only fix is memory that lives *outside* the conversation and is reloaded into every session.

A persistent store — written to automatically as you work, read back at the start of each session — turns the amnesiac into something continuous. Yesterday's hard-won understanding is there today, without you re-explaining it.

## Proof from the field

- **Before/after transcripts:** [`examples/`](../../examples/README.md) shows the same Day-2 session with and without the brain. The difference is not subtle.
- **It survives real failure.** The store behind this repo currently holds 300k+ entries across a dozen project wings. It has survived index corruption, an interrupted rebuild, a GPU-runtime swap, and multiple tool upgrades — because each failure was debugged, fixed, and several of the fixes went [back upstream](../../upstream/README.md) so the next operator does not hit them.

## Build it

1. Install [MemPalace](https://github.com/MemPalace/mempalace) (or an equivalent local memory store) and connect it to your runtime as an MCP server.
2. **Wire auto-save through hooks** — this is the part people skip and then wonder why memory is empty:
   - a **Stop** hook and a **pre-compaction** hook that write the session notes/state *before* context is lost;
   - a **session-start** hook that injects the recovered context back in.
3. **Keep it clean** (Pillar 1 applies to your own store): filter secrets/junk before ingest, dedup so repeated ingests do not bloat, and never let a noisy model write unreviewed "facts" into it.
4. Keep a tiny, human-readable index of what is stored, so both you and the assistant can see the memory shape at a glance.

## Operate it

- **One writer at a time.** Vector stores built on SQLite (FTS5 shadow tables) do not tolerate concurrent writers across processes — an ingest job and a live server writing the same store can corrupt the full-text index. Schedule ingest for when the server is idle, or gate it with a lock.
- **Verify the GPU is real.** "Device: GPU" in a status line is not proof; a silent CPU fallback runs ~10x slower with ~2 GB extra RAM per process. Load the actual model and check the *active* provider list — this failure mode is now fixed upstream ([PR ledger](../../upstream/README.md)).
- **Big stores change the physics.** At 300k+ entries, a full integrity check takes tens of seconds, and anything that runs it synchronously at startup will time out your MCP handshake. Measured, diagnosed, and fixed upstream: [MCP handshake case study](../../field-notes/2026-07-10-mcp-handshake.md).
- Back up before repairs; prefer surgical fixes (rebuild one index) over full rebuilds.

**Verify:** end a session, start a new one, and ask about something only the previous session knew. If it answers from memory without you re-explaining — the brain holds.

→ The failures you hit here become [field notes](../../field-notes/README.md), and the fixes become [upstream contributions](../../upstream/README.md). That loop is the system working as intended.
