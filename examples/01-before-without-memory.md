# Before — stateless chat, Day 2

*Synthetic transcript. Neutral scenario: a developer mid-way through refactoring a payments module across several days. No persistent memory. New session, next day.*

---

**You:** Okay, let's continue with the refactor.

**Assistant:** Happy to help! Could you tell me a bit about the project? What are you refactoring, what language/framework, and what have you done so far?

**You:** ...the payments module. We talked about all this yesterday.

**Assistant:** I don't have access to previous conversations, so I can't see yesterday's discussion. Could you recap: what's the current structure, what problem are we solving, and which approach did we decide on?

**You:** We agreed to split the `PaymentProcessor` god-class into a `GatewayClient`, a `RetryPolicy`, and a `LedgerWriter`, and to keep the old interface as a thin facade so nothing downstream breaks. We'd already done the `GatewayClient` extraction and written its tests.

**Assistant:** Got it, thanks for the recap. That's a sound approach. Before I suggest the next step — can you paste the current `PaymentProcessor` and the `GatewayClient` you extracted, so I can see the actual state and the conventions you're following?

**You:** *(pastes 300 lines)* ...this is the third time I've had to explain the retry-policy decision. Yesterday you even caught a subtle bug in how refunds interact with idempotency keys — that's gone now too, right?

**Assistant:** Unfortunately yes — I don't retain anything between sessions, so any specific bug or nuance from yesterday isn't available to me now. If you can describe what we found about refunds and idempotency keys, I'll pick it back up.

---

### What just happened

- ~15 minutes and a 300-line paste spent **reconstructing context the assistant already had yesterday**.
- The single most valuable artifact from Day 1 — the **subtle refunds/idempotency bug** the assistant itself discovered — is **gone**, and the user has to remember it well enough to re-surface it.
- Every day starts at a deficit. The assistant is brilliant and completely new, every morning.

This is the amnesia from [the problem](../docs/01-problem.md). Now read [`02-after-with-memory.md`](02-after-with-memory.md).
