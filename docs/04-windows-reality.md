🌐 **English** · [Deutsch](de/04-windows-reality.md)

[← Back to README](../README.md) · [Problem](01-problem.md) · [Thesis](02-thesis.md) · [Build Guide](03-build-guide.md) · **Windows Reality** · [FAQ](05-faq.md) · [In the Wild](06-in-the-wild.md)

---

# Windows Reality — the gaps nobody documents

Most agent tooling is written and tested on macOS/Linux. On Windows it mostly works — until it silently doesn't. These are real field notes from building the three pillars on Windows 11. None of this is a dealbreaker; all of it has a workaround. Knowing them up front saves you days.

## 1. "Continuous learning" captures but never distills

Some harnesses ship a learning loop: observe tool use → distill recurring patterns into reusable "instincts." On Windows the **capture half runs fine, but the distillation half can be gated off by design** (a headless sub-process that hangs non-interactively). Symptom: thousands of observations recorded, **zero** instincts produced — and it looks broken when it isn't.

**Workaround:** run the distillation manually — aggregate the observation log, pull out patterns that recur 3+ times, and write the instinct files yourself. The capture data is all there; you're just doing the step the daemon won't.

## 2. OAuth logins are not agent-drivable

Any MCP/tool whose auth is a **browser OAuth consent** (Gmail, calendar, etc.) cannot be completed by the agent. If you launch the auth flow from inside the agent's shell, it spawns a detached console you never see — it exits "successfully" while writing nothing.

**Workaround:** run the auth command yourself in a real terminal, click through the browser consent, then have the agent verify (check the credential file's modified-time changed, and make one live call). Re-auth, then reconnect the MCP server (it caches the dead token in memory).

## 3. `jq` and other Unix assumptions

Plenty of skill scripts assume `jq`, `find -printf`, or other GNU/Unix tools are present. On a stock Windows + Git-Bash setup they often aren't, and the script dies mid-run.

**Workaround:** install the missing tool, or do the step with a small Python/Node snippet instead. When a "deterministic" script fails on Windows, suspect a missing Unix dependency before assuming the logic is wrong.

## 4. GPU embeddings silently run on CPU

Install a vector store's `[gpu]` extra and you can end up with **both** the CPU and GPU runtime packages installed — and the CPU wheel shadows the GPU one. Result: it runs, just slowly, on the CPU, while you think it's on the GPU. On a memory backfill the CPU path can also ratchet RAM until the machine OOMs.

**Workaround:** after installing GPU support, explicitly uninstall the CPU runtime package and reinstall the GPU one; verify the GPU provider actually appears in the available-providers list before trusting it.

## 5. Git-Bash mangles native Windows commands

Running `.exe` tools through Git-Bash, two independent traps: MSYS rewrites `/flag` arguments into fake Windows paths (`taskkill /PID` → `…/Git/PID`), and Bash eats `$`-variables inside quoted `powershell -Command "..."` strings.

**Workaround:** for process/service/registry work, call PowerShell directly; for anything `$`- or `/flag`-heavy, write a script file and execute it rather than fighting inline quoting.

## 6. MCP servers time out at boot (it's contention, not breakage)

At session start, every MCP server + hook + (on Windows) logon-triggered scheduled task fires in the same second. Slow-to-import servers blow the default 30s startup window and get marked "failed" — even though they're healthy. Tell-tale: *several unrelated servers fail together*.

**Workaround:** raise the startup timeout in your runtime's env config, and remove the boot amplifier (don't trigger heavy work at logon/session-start). Prove a server is actually healthy with a direct handshake probe before declaring it broken.

---

**The point:** the three pillars absolutely work on Windows — this repo was built on it. But the polish is thinner than on Unix, and the failures are often *silent*. Treat "it ran and produced nothing" as a bug to investigate, not success.
