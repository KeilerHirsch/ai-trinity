[← Field Notes](README.md) · [Manual](../README.md) · [Upstream Ledger](../upstream/README.md)

---

# Case study: the 45-second MCP handshake (2026-07-10)

A worked example of the method — measure, eliminate, prove, fix, contribute. One day, from "VS Code will not even start" to two upstream PRs.

## Symptom

The memory server (Pillar 3, MemPalace as MCP over stdio) took **40–46 seconds** to answer the protocol `initialize` request. Claude Code hard-caps the MCP connect handshake at 60 s, so on a bad day — disk contention, a second process on the same store — the handshake starved and the server was marked dead. To the user it looked like "MemPalace hangs" and, combined with an unrelated config bug, like "VS Code is broken".

## What it was not (eliminated by measurement, not vibes)

| Hypothesis | Test | Verdict |
|---|---|---|
| Heavy Python imports | `python -X importtime` on the server module | ~1.5 s total (chromadb 0.85 s) — **not it** |
| Cold OS/disk cache | Re-run handshake probe warm | 46.15 s warm — **not it** |
| Eager embedder warmup | Check `MEMPALACE_EAGER_WARMUP` env | unset, off by default — **not it** |

The decisive instrument was a ~40-line stdio probe that spawns the server, timestamps every stderr line, and measures time-to-`initialize`-response. It showed: imports done at **1.45 s**, response at **20.45 s**. Nineteen seconds of *something* between the startup log line and the reply.

## The smoking gun

The server ran `PRAGMA quick_check` — a full SQLite integrity scan — **synchronously before answering the handshake**. Measured directly: **20.31 s** on this store (1.72 GB, 326k entries). Under lock/disk contention with a second MemPalace process: the observed 40–46 s. The `initialize` response itself never touches the database; it was purely queued behind the scan.

An upstream commit already skipped the check for stores >512 MB — but it was unreleased, and it trades away the integrity check entirely for exactly the stores that need it most.

## The fix

Answer `initialize` immediately; run the startup integrity/HNSW probes on a background thread. Tool calls that need the verdict serialize on a lock (double-checked, so the O(database size) scan never runs twice), and no tool result is served unverified. The HTTP transport in the same codebase already worked this way — the fix aligned stdio with existing precedent, which is also what made it an easy review.

**Result: 1.3–1.6 s handshake with the full 20-second integrity scan still running behind it.** Verified with the same probe that found the bug, plus two regression tests.

## Contributed upstream

- Async startup preflight → PR [#1987](https://github.com/MemPalace/mempalace/pull/1987)
- Found while fixing it: GPU DLL preload gap (silent CPU fallback) → PR [#1989](https://github.com/MemPalace/mempalace/pull/1989)

## Transferable lessons

1. **Measure before you patch.** Three plausible hypotheses died to two probe runs. The fix took less time than the speculation would have.
2. **Unit bugs travel in packs.** The same day's config debugging found hook timeouts set as `60000` — the field expects *seconds*, so that is 16.7 hours, not 60 s. Found one? Grep the whole file for its siblings (a fourth one was sitting two lines away).
3. **A handshake should never pay for background work.** If your protocol frontend blocks on maintenance, big-store users *will* time out — and it will look like your whole product is broken.
4. **The probe outlives the bug.** The 40-line timing probe is now the standard verification for every MCP change in this setup.
