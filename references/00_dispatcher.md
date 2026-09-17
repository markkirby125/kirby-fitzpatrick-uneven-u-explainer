# Uneven U Explainer — Technical Operational Dispatcher

**Framework Author**: William Fitzpatrick (*Writer Science*)  
**Source Lecture**: [The EXACT System to Transform Messy Drafts into Clear Writing](https://www.youtube.com/watch?v=6HPNb0tiDNg)  
**Parent Collection**: [Master Collection](../../kirby-fitzpatrick-writers-collection/SKILL.md) | [Global Help](../../../kirby-help/SKILL.md)  

---

## 1. Cognitive Foundation: Abstraction Altitude and the Hit-and-Run Snippet

A codebase is a Level 1 artifact. Every line in it is concrete, executable, and local; the abstraction that makes it *comprehensible* — the invariant, the lifecycle, the contract, the reason this mechanism was chosen over the three alternatives that were rejected — exists nowhere in the file. It exists only in the head of whoever wrote it. Documentation therefore has a structural bias: written without deliberate altitude control, it collapses downward to the altitude of the artifact it describes. The tutorial shows the code. The comment narrates the line. The PR body lists files. The reader gets syntax they can recite and a model they cannot reuse.

Eric Hayot's **Uneven U** is the correction. It specifies not just *what* to say but *at what altitude* each part of the explanation lives, and it enforces an order: descend through the levels to reach ground truth, then ascend back to a meaning the descent has now earned.

**The Altitude Scale (L1–L5).** Level 5 is the most abstract: the class of problem, the system property at stake. Level 1 is the most concrete: the exact data, code, trace, or measurement. Levels 4, 3, and (implicitly) 2 are intermediate registers between principle and evidence.

**The Uneven U Traversal.**

```text
L5 Concept  →  L4 Claim  →  L3 Mechanism  →  L1 Data/Code  →  L4 Meaning
  abstract        ↓              ↓                 ↓            ↑
              assertion      causal model      ground truth   transferable
                                                                  consequence
```

The shape is deliberately **uneven**, and the asymmetry carries the whole discipline:

- **The descent is long and fully travelled.** Every rung is crossed, in order, with no skipped levels. Each step down answers a question the rung above created.
- **The ascent is short and mandatory.** The return leg is one to three sentences, because re-teaching on the way up wastes the reader's attention. What the ascent must do is not repeat the mechanism — it must convert the mechanism into a *meaning* the reader can carry to the next module.
- **It is not a plateau and not a straight line.** Flat at L5 is hand-waving (*"we improved observability"*). Flat at L1 is the **hit-and-run snippet**: a code block that appears, runs in the reader's imagination, and departs without ever having been framed by a contract or repaid with a consequence.
- **The ascent stops at L4, not back at L5.** The reader now holds a verified mechanism, so the closing generalisation is bounded by what was shown. Returning to the opening conceptual altitude invites unearned closure — *"and that's the power of abstraction"* — which is the same defect as the hit-and-run snippet, inverted.

Three load-bearing definitions:

1. **Abstraction Altitude (A)** — the rung a given sentence occupies on the L1–L5 scale. Altitude is a property of *the sentence*, not of the writer's intent: *"the batcher flushes on a watermark"* is L3; *"the batcher is more responsive"* is L4-as-claim with no mechanism under it.
2. **Vertical Drift** — a mid-unit jump of two or more rungs with no bridging sentence. Drift is the mechanical cause of reader whiplash: L5 principle followed immediately by an unpreamble'd 40-line excerpt. The reader has no frame to hang the detail on, so the detail is filed as noise.
3. **Earned Abstraction** — a generalisation is legitimate only when the reader has already traversed the rungs beneath it *inside the same unit*. L5 vocabulary without L1 evidence is hand-waving; L1 evidence without L3 mechanism is cargo cult. Both are documentation defects, not stylistic preferences.

Why this binds harder in software than in prose: the artifact's own register exerts constant downward pressure, so the failure is systematic rather than accidental. The writer who is *in* the codebase — who has already paid for the model — routinely emits only the bottom rung and calls it an explanation. The on-call engineer at 03:00, the new hire in month one, and the author six months later cannot reconstruct the missing rungs from a snippet, because the snippet is exactly the part they already had.

```text
[ANTI-PATTERN: Flat-L1 — the hit-and-run snippet]

 reader altitude
   L5 |                                                        (never visited)
   L4 |                                                        (never visited)
   L3 |                                                        (never visited)
   L1 |#####:  #####  ####   #####:  ####  ##########################
      +----------------------------------------------------------------> time
       "Here's the code:"  block  block  block  "...and the migration."

 Reader leaves with: syntax that compiles in their head, no transferable model.
 Symptoms: copies the pattern into a context it doesn't fit; cannot say what
 breaks if the invariant is violated; asks "but why?" in the PR thread; the
 author re-explains it in a DM that never reaches the artifact.
```

```text
[PROTOCOL: The Uneven U — descend for evidence, ascend for meaning]

 reader altitude
   L5 |##  Concept: which system property is at stake   <- 1-2 sentences
   L4 |  ###  Claim: falsifiable statement about THIS   <- 1 sentence
   L3 |    #####  Mechanism: the causal machinery      <- 40-60% of the text
   L1 |       #####  Data/Code: minimum excerpt that   <- <= 30 lines, bounded
      |              proves the mechanism
   L4 |          ####  Meaning: consequence, cost axis, <- 1-3 sentences
      |                and the condition that reverses it
      +----------------------------------------------------------------> time
       descent earns the detail        ascent repays the attention

 Reader leaves with: the invariant, the mechanism, the evidence, and a
 generalisation that transfers to the next module unassisted.
```

The traversal is a **separate pass** from composition. Drafting is generative and permissive and will land flat at whatever altitude the author was standing on. The altitude audit happens on revision, per unit: *where does this paragraph enter the ladder, in which order does it cross the rungs, and where does it leave the reader standing?*

---

## 2. Core Transformation Protocols

1. **Never open at L1.** The first sentence of any walkthrough, docstring, or PR body names the system property at stake at L5 before any identifier appears. A reader who meets a function name before knowing why the function matters has to read the whole explanation twice — once to learn the subject, once to learn the point.
2. **Descend one rung at a time.** No jumps from L5 to L1. Every transition carries a bridging sentence that states *why* the explanation is going deeper: *"That bound only holds if the flush triggers promptly, so the trigger predicate is the load-bearing part:"* → then the code.
3. **Declare the claim before the mechanism it explains.** The L4 claim must be checkable *after* reading the L3 mechanism and the L1 excerpt, and must not be checkable without them. If the claim is obvious from the title, it is not a claim; if it cannot be checked at all, it is a slogan.
4. **Budget by altitude — the uneven allocation.** L5: 1–2 sentences. L4 claim: 1 sentence. L3 mechanism: 40–60% of the text. L1: the minimum excerpt that proves the mechanism (≤ ~30 lines; link the rest with `path:line` ranges). Closing L4: 1–3 sentences. Silence at a deficit rung is not brevity, it is an omission.
5. **Code is evidence, never explanation.** Every fenced block is preceded by the L3 mechanism sentence and followed by the L4 meaning sentence. An orphan code block — one with neither — is the hit-and-run defect, regardless of how self-documenting the code looks to its author.
6. **The ascent must not overshoot.** Return to L4 meaning, never to L5 generality. The close states the consequence, the cost axis it trades against, and the condition under which it stops being true. Ban closers of the form *"and that's the power of X"*, *"this makes us more scalable"*, *"clean and elegant"*.
7. **Name the L5 frame in the system's own vocabulary.** Type contract, invariant, lifecycle phase, failure mode, SLO, blast radius. *"Makes everything faster"* is not a frame: it names no property, so no mechanism can discharge it.
8. **Make the L4 claim observable, not adjectival.** *"Fetch count drops from 1,240 to 41 per reconcile"* is falsifiable and points at an artifact. *"Improves efficiency"* is neither. An L4 claim that no reader could verify has no business standing above L3.
9. **Every unit ends with an ascent.** A section, a docstring, a PR body, an ADR may not terminate on code. The last rung is always the meaning rung — that is what the reader takes on the way out.
10. **One U per unit.** A paragraph, a docstring, a section, a document each carry exactly one traversal. When the topic changes, a new U begins. Nesting Us mid-unit produces a collapsed middle, an L3 body that has lost its L5 frame and its L4 payoff.
11. **Ground the L1 excerpt in this codebase; make the L4 close portable.** Test the bottom of the U with *"does this excerpt name paths, lines, assertions, or measurements unique to this system?"* and the top of the ascent with *"would this sentence still be true and useful in a sibling module?"* A close that only restates the local mechanism has not ascended; it has merely stopped.
12. **Label every rung you could not reach.** When L1 is unavailable (proprietary code, redacted trace), state the evidence source at L3, mark the gap explicitly, and never substitute pseudocode dressed as a real excerpt. A declared gap is a fact; a fabricated rung is a defect that survives review.

### 2.1 The altitude ladder reference

| Rung | Cognitive register | Question it answers | Software artifact | Failure mode if omitted |
|---|---|---|---|---|
| L5 Concept | Abstract frame | *Why does this matter, and what class of problem is this?* | Invariant, contract class, SLO, architectural principle | Reader cannot triage; no reason to care about any detail below |
| L4 Claim | Assertion about this system | *What exactly is being claimed here, verifiably?* | Change statement, ADR decision line, docstring summary | Reader knows the subject but not the point; scans indefinitely |
| L3 Mechanism | Causal machinery | *How does it work, and why is that sufficient?* | Prose walkthrough, state transitions, sequence, gear description | Reader memorises without a model; cannot predict behaviour under change |
| L1 Data/Code | Ground truth | *What actually runs, and what did it measure?* | Fenced excerpt, `path:line`, test name, trace, benchmark output | Hit-and-run snippet; unverifiable claims; cargo-cult propagation |
| L4 Meaning (return) | Transfer | *So what — for this system and for the next one?* | Consequence, cost trade-off, blast radius, expiry condition | Knowledge dies at this file; every new reader pays the whole descent again |

### 2.2 Transformation table: anti-patterns and clean replacements

| Anti-Pattern | Altitude diagnosis | Clean Replacement |
|---|---|---|
| *"Here's the code:"* + 60-line block | Flat L1; no frame, no payoff | L5: *order is guaranteed per partition-key* → L4: *the consumer never re-reads a key* → L3: *single-writer partition assignment* → L1: the 12-line assignment excerpt → L4: *reordering across keys is still possible* |
| `// retry logic` above the retry block | L1 comment narrating L1 code — a rung the reader can already see | `// Bounds the caller's deadline, not the downstream service: 3 attempts x 400ms + jitter stays inside the 2s budget.` |
| Docstring: *"Returns the result."* | Restates the signature at L1 | `"""Failure is absorbing: the first error is returned and the stream closes, so callers may treat this handle as single-use."""` |
| *"We added a circuit breaker to improve resilience."* | Flat L5 — no claim, no mechanism, no evidence | L4: *50% error rate trips for 30s* → L3: *rolling window trips before the thread pool saturates* → L1: window size + `breaker.go:41` → L4: *a tripped breaker converts latency spikes into fast failures, at the cost of 30s of stale reads* |
| *"Line 4 initialises the client. Line 5 sends the request."* | Line narration — L3 register with zero causal content | *The client is built once per worker and reused; the request path only mutates headers.* |
| *"Optimised with memoisation."* | L4 claim with no rungs beneath it | L4: *recompute 1 per 4,000 calls* → L3: *keyed on the immutable config hash* → L1: the cache key + the hit-count assertion → L4: *invalid if the hash ever covers mutable state* |
| Tutorial that ends on a working snippet (*"…and you're done!"*) | Ascent never travelled | Closing L4: *the snippet assumes single-writer access; with concurrent writers the read-modify-write needs the version guard* |
| *"This is the reducer."* + block | L1 entered without the L5/L4 frame | L5: *state mutations must be replayable* → L4: *the reducer is pure over `(state, event)`* → L3: *no clock, no I/O, no id generation inside* → L1: the function → L4: *that purity is what makes time-travel debugging possible* |
| Pseudocode fenced as if it were source | Fabricated L1 | Label it: *"shape of the call, not the implementation"*, then cite the real `path:line` |
| *"Obviously this is O(n log n)."* | Unearned L5 vocabulary via intensifier | *Sorting dominates at n=100k (412ms of the 430ms total, `artifacts/sort-pprof.txt`); the scan below is linear.* |

### 2.3 Failure diagnostics

| Symptom in review | Altitude diagnosis | Fix |
|---|---|---|
| *"What does this function do?"* asked under a full explanation | L4 claim missing; the reader has L1 and L3 but no statement of purpose | Add the one-sentence claim immediately above the mechanism |
| *"Why do we even use this?"* | L5 frame omitted entirely | Open the unit with the invariant or system property at stake |
| *"I don't follow how this follows."* | Vertical drift L5 → L1 | Insert the bridging L4/L3 sentence; split the block, do not shrink it |
| Reader copies the pattern into an incompatible context | Ascent never travelled; the excerpt's preconditions were never stated | Ascend to L4 with the preconditions and the condition that reverses the decision |
| *"Is this actually true?"* | L4 claim with no L1 evidence under it | Bind claim → mechanism → measurement in the same unit |
| Doc reads fine but nobody changes the code confidently | L3 mechanism absent; the model exists only in the author's head | Replace line narration with gear-level causality |
| Comment thread argues about tone, not behaviour | Premises drifted to L5 generalities | Return to the L4 claim and the L1 artifact that decides it |
| Explanation is re-derived in every new PR | No written traversal to inherit | Persist the U in the artifact — docstring for the module, ADR for the decision |

**Related dispatchers.** Announce the ladder before descending it with [Cathedral Taxonomy](../../kirby-fitzpatrick-cathedral-taxonomy/SKILL.md); keep each rung chained to the next with [Linear Relay Linking](../../kirby-fitzpatrick-linear-relay-linking/SKILL.md); scale prose weight by altitude rather than evenness with [Cargo-Weighted Syntax](../../kirby-fitzpatrick-cargo-weighted-syntax/SKILL.md); strip the flat-L1 filler that pads the middle rung with the [Lexical Anti-Bloat Filter](../../kirby-fitzpatrick-lexical-anti-bloat-filter/SKILL.md); audit the finished traversal from a zero-context stance with the [Cold Reader PR Auditor](../../kirby-fitzpatrick-cold-reader-pr-auditor/SKILL.md).

---

## 3. Engineering Application Scenarios

### 3.1 Code Reviews — Reviewing the Traversal, Not Just the Diff

Two rules govern review. First, **a comment is itself a unit and gets its own U** — the reviewer's obligation to state the frame before the objection is the same obligation the author had. Second, **a reviewer can request a missing rung precisely**, because the ladder names what is absent: an L5 request is *"what invariant does this protect?"*, an L3 request is *"how does the delay schedule interact with the caller's deadline?"*, an L1 request is *"which test covers the deadline crossing?"*. Vague comments (*"this is unclear"*) are unaudited U traversals pointing at an unaudited U traversal.

**Before — a hit-and-run diff and a flat review.**

```go
// retry logic
if attempts > 3 {
    return err
}
```

> This retry logic is confusing and hardcoded. Please add clarity.

Diagnostics: the comment is flat at L4-as-judgment with no mechanism and no artifact; *"confusing"* names no rung; the author can only respond by guessing which altitude the reviewer wanted.

**After — reviewer names the missing rungs; author closes them in place.**

```markdown
**What the code is protecting (asking, not asserting)**
- L5: I read this as bounding *the caller's* deadline rather than shielding the downstream service. If that is the intent, the limit belongs to a budget, not a literal.
- L3: how does the backoff schedule compose with a slow attempt — does a 1.9s attempt consume the 2s budget, or is it abandoned mid-flight?
- L1: which test covers the deadline-crossing case? `retry_test.go` covers the attempt cap only.
```

```go
// RetryPolicy bounds the *caller's* deadline; it is not downstream protection.
// Worst case is the caller's budget: 3 attempts x 400ms backoff + 2 x 200ms jitter = 1.6s of 2s.
// The deadline check runs before each sleep, so a slow attempt consumes the budget
// instead of stacking retries on top of it.
// Consequence: a downstream outage surfaces as a caller-visible timeout in ~2s,
// not as a retry queue that outlives the request. Reverses if the budget becomes per-attempt.
// Evidence: retry_test.go:214 (deadline crossing), retry_test.go:231 (attempt cap).
```

**Acceptance rule for review comments.** A review comment that requests clarity must name the rung it is missing and the artifact that would close it. A comment that supplies a fix must traverse the U itself, or it hands the author the same defect one level up. Multi-line comments earn their length only where the logic is load-bearing — elsewhere, one L3 line: the budget in rule 4 applies to comments too.

### 3.2 PR Descriptions — A Complete Traversal in the Description Layer

The diff is L1 by construction. The PR body's job is the other four rungs: the property at stake, the claim, the mechanism, and the meaning. A body that lists files or reproduces the diff has performed a hit-and-run at document scale.

```markdown
## Why (L5 — the property at stake)
A write batcher's flush trigger defines its durability window. When the trigger is
time-only, the window is bounded in seconds but unbounded in records, so a burst
converts a latency guarantee into an unbounded buffer.

## What changed (L4 — the claim)
The flush predicate is now `len(buf) >= watermark || interval elapsed`, bounding
in-flight records at 512 regardless of arrival rate.

## How it works (L3 — the mechanism)
The loop previously awaited the ticker alone. At 4k records/s the 3s window
accumulated ~12k rows and reallocated the buffer on each growth step, so both write
lag and allocation churn scaled with load. `batcher.go:88` now checks the watermark
after every append and flushes as soon as the buffer reaches capacity, keeping the
slice at its initial capacity and bounding lag to `min(interval, 512 records)`.

## Evidence (L1 — ground truth)
batcher.go:88-104 · `make bench-batcher WORKLOAD=burst-4k RUNS=5`

| Metric | main @ 9f21c0d | PR head | Δ |
|---|---|---|---|
| p99 write lag | 3,140 ms | 610 ms | −81% |
| peak rows buffered | 12,480 | 512 | −96% |
| write txn / 10k rows | 42 | 59 | +40% (expected) |

## What this costs (L4 — meaning)
The watermark is a memory/latency dial, not a throughput dial: query count per row is
unchanged, and the measured +40% transaction count is the price of the smaller
batches. Any component holding an unbounded buffer behind a timer has the same defect
shape — the trigger, not the buffer, is the contract.
```

**Binding rules.** The heading text carries the rung, so a 15-second skim still crosses the U. The L4 claim must be checkable against the L1 table and nothing else. The L4 close states the cost axis, not a benefit adjective. If a rung has no artifact, it is labelled as an open question with an owner rather than written in the confident register.

### 3.3 Architecture RFCs / ADRs — The Uneven U at Document Scale

At document scale the traversal becomes a **rung map**: each section carries one altitude, and the order of sections is the order of the ladder. Two flat documents are equally defective — the all-L5 vision memo (no mechanism, no evidence, unfalsifiable) and the all-L4 decision log (commitments with no causal model and no artifacts, so nobody can tell whether they still hold).

| ADR section | Rung | Obligation |
|---|---|---|
| Context | L5 | The system property or invariant at stake, in the system's vocabulary |
| Decision | L4 | One falsifiable commitment about *this* system |
| Rationale | L3 | Why this mechanism works; why the rejected alternatives do not |
| Evidence | L1 | Benchmarks, traces, query counts, fixtures, `path:line` references |
| Consequences | L4 return | Cost axis, blast radius, the condition that reverses the decision, expiry |

**Rules for the rung map.**

- A section may not skip a rung. An ADR with Evidence and Consequences but no Rationale has a documented decision that nobody can reason about when conditions change.
- Consequences must not drift back to L5. *"This makes the platform more scalable"* is an unearned ascent; *"p95 ingest latency no longer scales with burst size; throughput per core is unchanged, and the trade is +40% transactions per row (measured, `bench-batcher`)"* is earned.
- Rejected alternatives live at L3 with their L1 evidence, or they are opinions the next reader will re-litigate from scratch.
- Long excerpts belong in an appendix; the body carries `path:line` ranges. An ADR whose evidence section is a 200-line paste has inverted the altitude budget.
- Onboarding docs traverse the same ladder once per subsystem, and each traversal ends at L4 — the new engineer needs the meaning rung most, because that is the rung they cannot reconstruct by reading code.

```markdown
# ADR-021 — Watermark-bounded write batching

## Context (L5)
A batcher's flush trigger *is* its durability contract: a time-only trigger bounds
latency in seconds and leaves record count unbounded.

## Decision (L4)
Flush on `min(interval, 512 records)` in `batcher.go:88`.

## Rationale (L3)
Ticker-only flushing reallocates the buffer on every growth step above 4k rows/s and
lets lag scale with arrival rate, because the ticker is independent of load. A
watermark makes the bound a function of the buffer, which is the only quantity the
writer controls.

## Evidence (L1)
`make bench-batcher WORKLOAD=burst-4k RUNS=5` · main @ 9f21c0d vs. PR head ·
p99 lag 3,140 ms → 610 ms · peak rows 12,480 → 512 · txn/row +40%.

## Consequences (L4 return)
Latency and memory are bounded by the watermark; throughput per row is unchanged, so
the +40% transaction count is the explicit price. Reverses if the storage layer
penalises small transactions more than the measured +40% (open, owner: storage, 30d).
Rollback is one commit; no schema or config migration.
```

---

## 4. Verification Checklist

- [ ] **Every code block is framed and repaid.** Each L1 excerpt is preceded by an L3 mechanism sentence and followed by an L4 meaning sentence — zero orphan blocks, and no excerpt longer than the minimum needed to prove the mechanism (longer regions cited as `path:line` instead of pasted).
- [ ] **The traversal is complete and unbroken.** Each unit opens at L5, crosses L4 → L3 → L1 in order with at most one rung per transition, and no bridging sentence is missing (no Vertical Drift from L5 to L1).
- [ ] **The ascent is earned and terminated at L4.** Every section, docstring, PR body, and ADR ends on the meaning rung with a consequence, a cost axis, and the condition that reverses it; no closer returns to L5 slogans (*"more scalable"*, *"cleaner"*, *"the power of X"*) and none ends on code.
- [ ] **The claim is falsifiable by the evidence beneath it.** Each L4 claim is observable and points at a verifiable artifact (metric, test name, `path:line`, command), and the prose agrees with the numbers below it.
- [ ] **Altitude budget holds and the ladder is grounded.** L3 occupies the bulk of the text, L5 and the closing L4 stay within 1–3 sentences each, the L1 excerpt is specific to this codebase, and the closing L4 is portable to a sibling module; any rung that could not be reached is explicitly labelled rather than approximated.