---
title: Deadlock Avoidance
type: concept
tags: [deadlock, avoidance, strategy, bankers-algorithm]
chapter: "03"
sources: [lecture-bs-3]
created: 2026-09-30
updated: 2026-09-30
---

# Deadlock Avoidance (Deadlock-Vermeidung)

## Definition

**Deadlock avoidance** leaves all four [[deadlock]] conditions in place but **examines every
allocation request before granting it**, and grants it only if the system would remain in a
[[safe-state|safe state]] afterwards.

## Context

Belongs to [[deadlocks]]. It sits between [[deadlock-prevention]] (static, absolute, restrictive)
and [[deadlock-detection]] (after the fact, needs [[deadlock-recovery]]): avoidance is dynamic and
never lets a deadlock happen at all.

## How it works

Two variants are given:

### 1. Denial of process initiation (Verweigerung des Prozessstarts)

A new [[process]] is only admitted if the system can still satisfy the **maximum claims of all
processes including the new one**. Simple, but it keeps the degree of **concurrency low** —
processes are refused admission long before the resources are actually scarce.

### 2. Denial of resource allocation — the [[bankers-algorithm|banker's algorithm]]

For each individual request:

1. Pretend the request is granted and compute the hypothetical state.
2. Run the safety test — the [[deadlock-detection-marking-algorithm]] — on that hypothetical
   state.
3. If the result is **[[safe-state|safe]]**, grant the request. If it is **unsafe**, make the
   requesting process wait, even though the resources are physically available.

The crucial insight is that an **unsafe state is not a deadlock** — it is a state from which a
deadlock *could* become unavoidable. Avoidance refuses to enter it at all.

### Prerequisites

- Every process must declare its **maximum resource claim in advance** (the matrix **A**).
- The number of processes and resources must be fixed.
- Processes must eventually return everything they take.

These requirements are why avoidance is rare in real general-purpose systems: programs usually do
not know their maximum needs.

## Related

- [[bankers-algorithm]] — the concrete procedure.
- [[safe-state]] — the criterion it tests for.
- [[deadlock-detection-marking-algorithm]] — the safety test, reused.
- [[deadlock-prevention]] — the static alternative.
- [[deadlock-detection]] — the reactive alternative.

## Common confusions

- **Avoidance ≠ prevention.** Prevention makes a condition impossible once, at design time;
  avoidance leaves everything possible and refuses individual requests at runtime. This is a
  standard exam distinction.
- **Unsafe ≠ deadlocked.** An unsafe state may well run to completion if the processes happen not
  to claim their maxima. Avoidance is deliberately pessimistic.
- **Avoidance uses the same algorithm as detection**, but applies it to a *hypothetical future*
  state rather than the *actual current* one.

## Sources

- [[lecture-bs-3]] (p17, p21)
