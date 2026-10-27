---
title: Banker's Algorithm
type: algorithm
tags: [deadlock, avoidance, algorithm, safe-state]
chapter: "03"
sources: [lecture-bs-3]
created: 2026-09-30
updated: 2026-09-30
---

# Banker's Algorithm (Bankiersalgorithmus)

## Purpose

Implement [[deadlock-avoidance]]: decide, for each individual resource request, whether granting
it would leave the system in a [[safe-state|safe state]]. If yes, grant it; if no, make the
requesting [[process]] wait — **even though the resources are physically available**.

## Idea

The banker metaphor: a banker has a fixed amount of capital and several clients, each with an
agreed **credit line** (their maximum claim). The banker may lend freely as long as it can still
guarantee that **every** client can eventually be served up to their credit line and repay. A loan
that would make that guarantee impossible is refused, even if the cash is on hand.

The state is kept **safe** at all times, so a deadlock can never arise — no detection and no
[[deadlock-recovery]] is ever needed.

## Procedure

Uses the same structures as [[deadlock-detection-marking-algorithm]]: resource vector **R**,
allocation matrix **Z**, maximum-claim matrix **A**, availability vector **V**.

When process *i* requests a vector **q**:

1. **Validity check** — if `q > (A − Z)ᵢ`, the process has exceeded its declared maximum: error.
2. **Availability check** — if `q > V`, the resources are not there: the process waits.
3. **Hypothetical grant** — compute the state that *would* result:
   ```
   V'  = V  − q
   Zᵢ' = Zᵢ + q
   ```
4. **Safety test** — run the [[deadlock-detection-marking-algorithm]] on `(R, V', Z', A)`.
   - If **all processes can be marked**, the hypothetical state is **[[safe-state|safe]]** →
     **grant** the request for real.
   - If **any process stays unmarked**, the state is **unsafe** → **undo** the hypothetical grant
     and **block** the requester.

The variant for whole processes is **denial of process initiation**: admit a new process only if
the maximum claims of all processes *including* the new one can still be satisfied. It is simpler
but keeps concurrency low.

## Complexity / cost

- One full safety test **per request** — **O(n² · m)** each time; considerably more expensive than
  [[deadlock-detection]], which runs only occasionally.
- Requires every process to **declare its maximum claim in advance**, which real programs usually
  cannot do. This is the main reason the algorithm is rarely used outside teaching.
- Requires a **fixed** number of processes and resources; processes must eventually return
  everything.
- **Pessimistic**: unsafe is not the same as deadlocked, so the algorithm refuses some requests
  that would in fact have been harmless — reducing utilisation and concurrency.

## Related

- [[deadlock-avoidance]] — the strategy it implements.
- [[safe-state]] — the criterion it preserves.
- [[deadlock-detection-marking-algorithm]] — reused verbatim as the safety test.
- [[deadlock-prevention]] — the static alternative; preclaiming is the crude version of the same
  information requirement.
- [[deadlock]] — what it makes impossible.
- [[resource]] — what is being allocated.

## Common confusions

- **Unsafe ≠ deadlocked.** The algorithm blocks on *unsafe*, which is strictly stronger than
  avoiding deadlock. See [[safe-state]].
- **It is not detection.** Detection inspects the **actual current** state after the fact;
  the banker's algorithm inspects a **hypothetical future** state before committing.
- **It is not preclaiming.** Both need the maximum claims, but preclaiming
  ([[deadlock-prevention]]) hands out everything at once at process start; the banker's algorithm
  hands out resources incrementally.

## Sources

- [[lecture-bs-3]] (p21)
