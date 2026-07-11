[← Field Notes](README.md) · [Manual](../README.md) · [Upstream Ledger](../upstream/README.md)

---

# In the Wild: a live cockpit on all three pillars

## GSOC Command Center — the Trinity, watching itself

**Problem:** Each pillar produces state worth seeing at a glance — the persistent brain's contents (Pillar 3), the model's outbound work and the feedback it draws (Pillar 1, via GitHub), and the machine actually running all of it. But there is no single surface that holds the whole picture *live*. You end up bouncing between a memory search, a browser tab full of notifications, and a terminal, reconstructing the situation by hand every time.

**What it does:** a localhost dashboard with four panels — a **MemPalace** panel that reads the persistent brain directly and runs full-text search over the entire memory; a **GitHub cowork** panel that separates real human feedback (comments, reviews, replies on your threads) from CI noise and flags forks that have drifted behind upstream; an **active fronts + deadlines** panel; and a **system-health** panel (GPU, disk, repo state, memory freshness).

**How, briefly:** the memory panel opens ChromaDB's own sqlite file **read-only** and queries its FTS5 index directly — no Chroma server, no embedding model, so it never contends with the writer that keeps the brain fed. The GitHub panel reuses the classification from a companion PowerShell monitor. It binds to localhost only, is air-gapped (no external sources — a Content-Security-Policy enforces it), every endpoint is a side-effect-free read, and every piece of untrusted text (issue titles, comment bodies, memory snippets) is HTML-escaped before it reaches the page.

**Why it belongs here — but stays unpublished:** unlike the other tools in these notes, this one is deliberately *not* released. It reads a live, private memory store and inbox — it's a cockpit, not a portfolio piece, and publishing it would map an author's private infrastructure to no one's benefit. It earns a field note anyway because it is the sharpest demonstration of the thesis: **Pillar 3 is not a black box you hope is working — it is a first-class, directly-queryable data source**, and a tool that treats it as one becomes possible. And it was built to the same Pillar 1 + 2 bar it observes: a full static-analysis battery plus two independent review agents (code + security) had to come back with no critical or high findings, and coverage sat near 90%, before it was allowed to ship. The Trinity, in other words, built the instrument that now watches the Trinity.

---

Have you built something because of this thesis? Open an issue — see [CONTRIBUTING](../CONTRIBUTING.md).
