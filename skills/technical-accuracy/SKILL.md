---
name: technical-accuracy
description: Run before any EEZ content is finalised — checks draft against 5 technical distinctions that must not be blurred. Use after alliance-outreach, content-ingestion, or gip-authoring produces a draft.
---

# Technical Accuracy Skill

Pre-publish gate for EEZ content. Checks any draft that touches EEZ technical claims against five distinctions that have been blurred in the past. A single FAIL blocks approval.

## Trigger

Run this skill whenever a draft:
- Mentions finality, speed, or transaction throughput
- Describes how EEZ chains move state or assets cross-chain
- Describes EEZ's proving or security model
- Uses the words "L2", "layer", "bridge", "transaction", or "ZK prover"
- Is produced by `alliance-outreach`, `content-ingestion`, or `gip-authoring` and will be sent externally

Also run on any draft that Armagan flags as "check the technical claims."

---

## The 5 Distinctions

---

### 1. Async path vs native rollup finality

**Correct:** EEZ has two finality paths. The async path settles in ~20 minutes. Native rollups settle in ~12 seconds (same block as the parent chain). These are different guarantees for different use cases.

**Common wrong version:** "EEZ has fast finality" or "EEZ settles in 12 seconds" — applied without specifying which path.

**Pass criterion:** If the draft mentions finality or speed, it must name the path (async or native rollup) alongside the figure. A claim that applies a single finality number to EEZ as a whole fails.

---

### 2. Execution entries, not transactions

**Correct:** EEZ native rollups process "execution entries." This is the correct technical term. It is not an alias for "transaction."

**Common wrong version:** "Transactions are submitted to EEZ" or "each transaction in the rollup."

**Pass criterion:** Draft uses "execution entries" when describing operations processed inside an EEZ native rollup. "Transaction" is acceptable only when referring to the L1 layer or to a partner chain's own model — not to EEZ-native operations.

---

### 3. Proxies, not bridges

**Correct:** Cross-chain movement in EEZ is handled via proxies. Proxies are synchronous and share state — there are no extra trust assumptions. Bridges are asynchronous and carry their own trust model.

**Common wrong version:** "EEZ bridges assets between chains" or "the EEZ bridge."

**Pass criterion:** Draft does not use the word "bridge" to describe EEZ's cross-chain mechanism. "Proxy" or "proxies" is used instead. If the draft discusses a third-party bridge (e.g. a partner's own bridge), that reference is clearly scoped to the external party and not attributed to EEZ.

---

### 4. Multi-prover, not single-prover

**Correct:** EEZ's architecture supports multiple proof systems in parallel. No single ZK prover is the authoritative one.

**Common wrong version:** "EEZ uses a ZK prover" or "the EEZ ZK proof system" as if there is one.

**Pass criterion:** Draft does not describe EEZ as having "a prover" or "the prover." If proving is mentioned, the multi-prover model is acknowledged — either explicitly ("multiple proof systems") or by avoiding singular/definite framing.

---

### 5. Economic zone framing, not L2 framing

**Correct:** EEZ is an economic zone built on Ethereum. It is not "a Layer 2." Calling it an L2 flattens the positioning and misrepresents the architecture and mission.

**Common wrong version:** "EEZ is an L2," "EEZ is a Layer 2 network," "the EEZ L2."

**Pass criterion:** Draft does not use "L2" or "Layer 2" as a description of EEZ itself. References to EEZ using L2 infrastructure (e.g. "native rollups are a type of L2 construction") are acceptable if they frame EEZ as something built on top of, not equivalent to, that construction.

---

## Eval Format

Run each distinction in order. Output:

```
[ASYNC vs NATIVE FINALITY]
Status: PASS | FAIL | N/A
Issue: (if FAIL) quote the draft's incorrect claim
Fix: (if FAIL) rewritten sentence that is correct

[EXECUTION ENTRIES]
Status: PASS | FAIL | N/A
Issue: (if FAIL) quote the draft's incorrect claim
Fix: (if FAIL) rewritten sentence that is correct

[PROXIES NOT BRIDGES]
Status: PASS | FAIL | N/A
Issue: (if FAIL) quote the draft's incorrect claim
Fix: (if FAIL) rewritten sentence that is correct

[MULTI-PROVER]
Status: PASS | FAIL | N/A
Issue: (if FAIL) quote the draft's incorrect claim
Fix: (if FAIL) rewritten sentence that is correct

[ECONOMIC ZONE NOT L2]
Status: PASS | FAIL | N/A
Issue: (if FAIL) quote the draft's incorrect claim
Fix: (if FAIL) rewritten sentence that is correct
```

---

## Overall Verdict

After all five evaluations:

```
VERDICT: APPROVED
All checked distinctions pass. Draft is cleared for Armagan's review.
```

or

```
VERDICT: NEEDS REVISION
[N] distinction(s) failed. Apply the fixes above before the draft proceeds.
```

APPROVED means all five are PASS or N/A. A single FAIL produces NEEDS REVISION regardless of the other four.

---

## Data Sources

| Source | Used for |
|---|---|
| `knowledge/eez/kb/02-technical/Technical-Architecture.md` | Architecture overview, native rollup model |
| `knowledge/eez/kb/02-technical/Synchronous-Composability.md` | Proxy model, same-block composability |
| `knowledge/eez/kb/02-technical/ZK-Real-Time-Proving.md` | Multi-prover architecture |
| `knowledge/eez/kb/02-technical/EEZ-Spec-Walkthrough-Blog.md` | Finality paths, execution entries |
| `knowledge/eez/kb/02-technical/EEZ-Smart-Contract-Technical-Deep-DIVE.md` | Full technical reference |
| `knowledge/eez/03-technical-spec.md` | Canonical spec (cached) |

When a FAIL produces an ambiguous correction, read the relevant doc above before writing the Fix line. Do not invent technical corrections from memory.

---

## Rules

- N/A is valid when the draft genuinely does not touch that distinction. Do not fail content for not discussing something.
- Do not fail a draft that correctly avoids a topic — silence on a distinction is not a violation.
- Quote the draft's exact text in the Issue field. Do not paraphrase.
- The Fix must be a drop-in replacement, not a note to the author. Write the corrected sentence.
- This skill does not check style, voice, or partner framing — that is `alliance-outreach` scope. Scope here is technical accuracy only.
- If the draft is too ambiguous to evaluate a distinction (e.g. finality mentioned but context unclear), mark FAIL with Issue noting the ambiguity. Ambiguity in a public draft is itself a problem.
