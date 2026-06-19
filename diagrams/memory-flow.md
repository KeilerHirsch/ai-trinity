# Diagrams

## The three pillars

```
                    ┌─────────────────┐
                    │  MODEL QUALITY  │   error rate screened
                    │  (pillar 1)     │   → trustworthy inputs
                    └────────┬────────┘
                             │
              ┌──────────────┴──────────────┐
              │                             │
     ┌────────┴────────┐          ┌─────────┴─────────┐
     │   FOUNDATION    │          │ PERSISTENT BRAIN  │
     │   (pillar 2)    │◀────────▶│   (pillar 3)      │
     │ skills/hooks/   │          │ external memory,  │
     │ agents/gates    │          │ auto-saved        │
     └─────────────────┘          └───────────────────┘
        method/discipline            continuity
```

All three must hold at once. Remove any one and the system degrades (see [thesis](../docs/02-thesis.md)).

## Memory flow — how continuity actually happens

The persistent brain is wired through runtime hooks. This is the loop that defeats the amnesia:

```
   SESSION N                                   SESSION N+1
   ─────────                                   ───────────
   [session-start hook]                        [session-start hook]
        │ loads context from store                  │ loads context (incl. what N saved)
        ▼                                            ▼
   ... you work ...                             ... you continue, no re-explaining ...
        │                                            │
   [pre-compaction hook] ──┐                    [pre-compaction hook] ──┐
   writes state before     │                    writes state before     │
   context is dropped      ▼                    context is dropped       ▼
   [stop hook] ────────▶ ┌──────────────────┐  [stop hook] ──────────▶ ┌──────────────────┐
   writes session notes  │  PERSISTENT      │  writes session notes    │  PERSISTENT      │
   + structured memory   │  STORE (local)   │  + structured memory     │  STORE (local)   │
                         │  filtered, dedup │                          │  filtered, dedup │
                         └──────────────────┘                          └──────────────────┘
```

Key points:
- **Auto-save, not manual.** The Stop and pre-compaction hooks fire without you asking, so nothing depends on remembering to save.
- **Reload at start.** The session-start hook injects the recovered context, so session N+1 begins where N ended.
- **Clean on the way in.** Filter secrets/junk and dedup before storing — a noisy store launders bad data into "remembered fact" (pillar 1 applies to your own memory).
