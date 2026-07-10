[← Manual](../README.md) · [Problem](00-problem.md) · [Thesis](01-thesis.md) · [Pillars](pillar-1-model/README.md) · [Windows](02-field-conditions-windows.md) · [FAQ](03-faq.md) · [Field Notes](../field-notes/README.md) · [Upstream](../upstream/README.md)

---

# FAQ

**Is this a product I install?**
No. It's a thesis plus a build path. The three pillars are independent open-source projects; this repo is the map that connects them and the field notes from running them.

**Why not just use longer context windows?**
A bigger window delays the amnesia, it doesn't remove it. Compaction still happens, and detail still gets dropped — it just happens later. Persistent external memory is a different mechanism, not a bigger buffer. See [the problem](00-problem.md).

**Isn't "handoff notes / summarize for next time" enough?**
No. A summary is lossy by definition; a handoff is a summary of a summary. The detail that made the last session valuable is exactly what a handoff drops. You want the session *preserved and reloaded*, not compressed into a paragraph.

**Do I have to use these three specific projects?**
No. They're the references that work well together, but the thesis is tool-agnostic: screen your model's error rate, put a disciplined harness around it, and give it persistent external memory. Swap in equivalents if you prefer — just don't drop a pillar.

**Why is model error rate treated as pillar one?**
Because memory + method built on a model that confidently invents facts gives you a permanent, well-organized archive of nonsense. The error rate is the foundation the other two pillars sit on. See [thesis, pillar 1](01-thesis.md).

**Where's the proof it works?**
In [`examples/`](../examples/README.md): before/after transcripts showing a session that started with no context vs. the same start with the persistent brain loaded. Claims are cheap; the transcripts are the point.

**Does this only work on Windows / only on Claude?**
It was built on Windows with Claude Code, but the thesis is general. Windows users should read [Windows Reality](02-field-conditions-windows.md) for the platform-specific gaps and workarounds.

**Is my data safe / does anything leave my machine?**
The persistent-brain pillar is a *local* store by design — nothing in this approach requires exposing your memory to the public internet. How you run it is up to you; keep it local.

**Who maintains this?**
KeilerHirsch. Contributions and corrections welcome via issues/PRs.
