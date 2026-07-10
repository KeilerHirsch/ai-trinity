[← Field Notes](README.md) · [Manual](../README.md) · [Upstream Ledger](../upstream/README.md)

---

# In the Wild: tools built on this

The three pillars are not just theory — they generate real, specific engineering problems once you actually try to live inside them. This page tracks concrete tools born directly from following this thesis, starting with the author's own.

## Schrödinger Sync — closing a real gap in Pillar 3

**Problem:** Pillar 3 (the persistent brain) only helps with what actually reaches it. Claude Code's local session transcripts land on disk as JSONL — trivial to ingest. Claude Desktop and Claude's web app do not: those conversations live server-side, with no local file a hook can watch. Every Desktop/Web session was, by construction, invisible to the memory store — a second amnesia hiding behind the first one this repo already solves.

**What it does:** [Schrödinger Sync](https://github.com/KeilerHirsch/schroedinger-sync) exports your own claude.ai conversations, project knowledge docs, and the memory feature's content to local Markdown — closing exactly that gap, so Desktop/Web history feeds the same persistent brain Claude Code already writes to.

**How, briefly:** claude.ai sits behind a Cloudflare managed JS challenge that headless automation cannot clear. The tool decrypts your own session cookie from Claude Desktop's local, DPAPI-protected store, then drives a real, *visible* Chrome window via the Chrome DevTools Protocol — Chrome clears the challenge itself, and the tool then makes same-origin API calls from inside that page. No credential ever touches a third party; see the tool's own [SECURITY.md](https://github.com/KeilerHirsch/schroedinger-sync/blob/main/SECURITY.md) for the full threat model, including why "it decrypts a DPAPI cookie store" is deliberately not glossed over.

**Why it belongs here, not just in its own repo:** it's a working demonstration of Pillar 3's actual bar — persistence that survives a session, an app switch, and a platform's own anti-automation defenses, not a toy example. Windows-specific, MIT-licensed, built with the same "own account only, no covert operation, tests enforce the invariants" discipline this repo argues for.

---

Have you built something because of this thesis? Open an issue — see [CONTRIBUTING](../CONTRIBUTING.md).
