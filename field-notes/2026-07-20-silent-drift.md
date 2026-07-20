[← Field Notes](README.md) · [Manual](../README.md) · [Upstream Ledger](../upstream/README.md)

---

# Case study: the config that lied (2026-07-20)

The setup stopped working after a tooling update. The obvious read was "an update wiped my
context, everything has to be rebuilt and re-indexed." That read was wrong, and acting on it
would have cost a week. What actually happened: nothing was lost. Things had become *quietly
incorrect*, and no part of the system was watching for that.

One day, four independent incidents, one mechanism.

## Symptom

An agent harness with 34 hooks, ~220 memory files, and a plugin layer had drifted far enough from
reality that routine work started failing in confusing ways: a repository was not where the notes
said it was, a safety gate approved a push it should have examined, and an update command reported
success while changing nothing.

The felt experience — *"after an update it knows nothing and everything must be wired again"* — is
the one that misleads. It points at rebuilding. Rebuilding is the expensive answer to the wrong
question.

## What it was not (eliminated by measurement, not vibes)

| Hypothesis | Test | Verdict |
|---|---|---|
| Plugin config pinned to a retired model generation | grep `model:` frontmatter across all 67 agents | every one uses an *alias* (`sonnet`/`opus`/`haiku`), which resolves at runtime — **not it** |
| Stale model IDs somewhere in the executable path | grep model strings in `*.json/js/sh/py`, excluding docs | hits lived only in a *different* harness's config and an unreferenced subpackage — **not it** |
| Permission prompts caused by an allowlist gap | inspect all 204 rules | `grep`, `head`, `echo`, `cat`, `ls` were all present and correct — **not it** |
| The recurring cost warning is a built-in CLI feature | grep the literal message across every config directory | it is a *plugin hook* with a documented off-switch; the earlier "it's native" conclusion came from a result list truncated at 30 of 175 matches — **not it** |

Four plausible causes, four measurements, four deaths. The last one is the instructive failure:
the prior session had run the correct search and drawn the wrong conclusion, because the one file
that mattered sat at position ~172 in an unsorted result list dominated by session transcripts.
**An "X exists nowhere" claim is only valid if the search could not have been truncated.**

## The smoking gun

Nothing in the system compared what the documentation said against what was actually true. Four
findings, all with the same shape — *confidently wrong, and silent about it*:

1. **A package manager that reports success and does nothing.** `plugin update` answered
   "already at the latest version (2.0.0)" while the installed copy was 33 commits behind
   upstream. The check compares a version *string*; the project ships a rolling `main` without
   bumping it. Verified by content, not by the tool's own report: an agent definition read
   `model: sonnet` locally and `model: haiku` upstream.

2. **Documentation surviving a migration that it did not describe.** All repositories had moved
   to a different drive weeks earlier. A scan of every memory file for referenced filesystem
   paths found the old locations still documented in ~10 places — including an **emergency
   runbook** whose recovery script no longer existed at the path it named. A runbook is read
   during an incident. That is the worst possible moment to discover the instructions are stale.

3. **A protection layer scoped to the wrong place.** 29 of 34 hooks were registered
   project-locally — including every anti-slop gate and the secret-leak guard. The actual code
   lived in *other* workspaces. The guards had never once fired where the pushes happened.

4. **An ownership check written as `==`.** The gold-standard gate compared a repository's owner
   against a single hardcoded username. Every repo owned by the *organization* — including the
   most important one in the fleet — fell through the comparison and was waved past unexamined.
   The same bug hid those repos from the fleet audit and made its API calls 404 into empty facts,
   which then "passed" every check.

Each of these produced a green light. None produced a warning.

## The fix

**A drift watchdog, not a rebuild.** At session start, every filesystem path referenced anywhere
in the memory layer is checked for existence; anything missing is reported loudly. Read-only,
fail-open, silent when clean. It fired on its first real boot and has been the source of every
correction since.

Two design details did the actual work:

**Noise reduction had to be structural, not suppression.** The naive first scan reported 205 dead
paths — mostly garbage. Silencing them with an ignore list would have destroyed the signal. Each
false-positive *class* got a rule instead: strip URLs before matching (a `https://…` cut at the
scheme looks exactly like a drive path), a regex lookbehind so `HKLM:\…` is not read as drive
`M:`, and "longest existing ancestor" checks that recognise German noun compounds gluing a path
into a word and truncation at a space (`C:\Program` ← *Program Files*). Result: **205 → 25**, and
the 25 are real.

**The watchdog needs its own watchdog.** It writes a heartbeat file on every run. Without one it
could fail silently — which is precisely the failure it exists to catch.

## The trap this walked into twice

Two of the day's "findings" were false alarms produced by the diagnostic probes themselves. A
probe guessed a module name from a variable alias and reported a working safety gate as dead code.
A second probe built test fixtures with invented dictionary keys, so every lookup returned `None`,
no rule fired, and a functioning gate looked inert.

**A broken probe and dead production code produce identical symptoms: nothing fires, everything
passes.** The probe is newer and less tested than the thing it measures — suspect it first. The
correction was to make the test self-invalidating: five cases with *expected* verdicts
(broken→deny, permissive-license→deny, clean→allow, private→allow, unverifiable→warn). `5/5` is
evidence. A lone `allow` never was.

## Transferable lessons

1. **The failure mode is silent wrongness, not knowledge loss.** A missing fact makes you search.
   A wrong fact makes you act. Wrong is more expensive than absent — and only wrong is invisible.

2. **A tool's success message is not evidence.** "Already up to date", "allow", exit code 0 — each
   was produced by something that had done nothing at all. Verify by content, on the artifact.

3. **Ask of every component: what notices if this becomes silently wrong?** If there is no answer,
   the component is unfinished, however well it works today. This is the cheapest question in the
   whole method and it found all four incidents.

4. **Scope is a security property.** A guard registered in the wrong place is worse than no guard,
   because it produces the feeling of protection. Verify *where* it fires, not just *that* it does.

5. **Suppression and filtering are not the same thing.** Both shrink the output. Only one keeps
   the signal.
