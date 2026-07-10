[← Back to README](../README.md) · [Manual](../manual/00-problem.md) · [Field Notes](../field-notes/README.md)

---

# Upstream Ledger — closing the loop

The thesis has a fourth movement that only shows up once you live it: **operating the pillars daily generates real bugs in the pillars themselves — and the method (Pillar 2) is what turns those into fixes the next operator inherits.** This page is the receipts: every contribution this setup has sent back to its own foundations.

## Why this matters

Anyone can recommend three projects. Running them at real scale — a 300k-entry store, Windows, GPU embeddings, daily sessions — surfaces failures the projects' own CI never sees. Fixing them upstream is both the honest test of the thesis (would you contribute to a foundation you didn't trust?) and the strongest proof it holds.

## Ledger — [MemPalace](https://github.com/MemPalace/mempalace) (Pillar 3)

| Date | PR | What | Status |
|------|----|------|--------|
| 2026-07-10 | [#1989](https://github.com/MemPalace/mempalace/pull/1989) | Preload CUDA/cuDNN DLLs before the first GPU session — kills the silent CPU fallback (GPU claimed, CPU delivered, ~10x slower) | open |
| 2026-07-10 | [#1987](https://github.com/MemPalace/mempalace/pull/1987) | Answer MCP `initialize` immediately; run the O(database-size) startup integrity scan on a background thread. Handshake: 20–46 s → **1.3 s** ([case study](../field-notes/2026-07-10-mcp-handshake.md)) | open, CI green |
| 2026-07-07 | [#1956](https://github.com/MemPalace/mempalace/pull/1956) | Bounded retry before the MCP writer lease goes permanently read-only | **closed by author** — a cleaner fix (#1960, per-call self-heal) landed first; verified it covers the motivating case and closed with credit. Knowing when *not* to merge your own fix is part of the method. |
| 2026-07-07 | [#1945](https://github.com/MemPalace/mempalace/pull/1945) | Atomic archive rename in repair — a live-reproduced **data-loss** bug (interrupted `copytree`+`rmtree` fallback left a half-destroyed store) | **merged** |
| 2026-07-02 | [#1910](https://github.com/MemPalace/mempalace/pull/1910) | Windows per-file mine-lock gives up after ~10 s instead of blocking — waiter crashes on large files | open |
| 2026-07-02 | [#1909](https://github.com/MemPalace/mempalace/pull/1909) | Strip the full Claude Code slash-command envelope + ANSI escapes from ingested transcripts (fixes [#1333](https://github.com/MemPalace/mempalace/issues/1333)) | open |

Plus filed issues with full repro data: [#1990](https://github.com/MemPalace/mempalace/issues/1990) — self-verifying saves (a memory that fails silently is not yet memory; proof-of-concept watchdog included), [#1908](https://github.com/MemPalace/mempalace/issues/1908) — MCP tools hang after an interrupted mine (chromadb compactor).

## Tools born from the thesis

- [Schrödinger Sync](https://github.com/KeilerHirsch/schroedinger-sync) — Desktop/Web conversations were invisible to the persistent brain; this closes the gap. ([field note](../field-notes/schroedinger-sync.md))

## The loop, explicitly

```
live the system  →  hit a real failure  →  field note (diagnose with method)
       ↑                                            ↓
  everyone benefits  ←  upstream PR  ←  fix verified locally
```

If you run this stack and hit something new: that is not an annoyance, that is the loop starting. Write it down, fix it, send it up.
