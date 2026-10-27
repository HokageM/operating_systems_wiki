---
title: Safe State
type: concept
tags: [deadlock, avoidance, safe-state]
chapter: "03"
sources: [lecture-bs-3]
created: 2026-09-30
updated: 2026-09-30
---

# Safe State (sicherer Zustand)

## Definition

A system state is **safe** if there exists **at least one sequence in which all processes can be
run to completion**, each receiving its maximum remaining claim from the resources that are free
plus those released by its predecessors in the sequence.

A state that is not safe is **unsafe** (unsicherer Zustand).

## Context

Belongs to [[deadlocks]]. The safe state is the criterion that [[deadlock-avoidance]] and the
[[bankers-algorithm]] are built on.

## How it works

Safety is tested with the [[deadlock-detection-marking-algorithm]]: repeatedly find a process
whose **outstanding need `A − Z`** fits into the current availability vector **V**, pretend it
finishes, add its allocation back to V, and mark it. If every process can be marked, such a
sequence exists and the state is **safe**.

The sequence found is the **safe sequence**; in the first worked example of exercise 3-5 it is
P5, P4, P1, P3, P2.

### Safe, unsafe, deadlocked

```
        ┌─────────────────────────────────┐
        │            unsafe               │
        │   ┌──────────────────────┐      │
        │   │      deadlocked      │      │
        │   └──────────────────────┘      │
        └─────────────────────────────────┘
   ┌───────────────────────────────────────────┐
   │                  safe                     │
   └───────────────────────────────────────────┘
```

- **Safe** ⟹ no deadlock now and none possible if allocations are controlled.
- **Unsafe** ⟹ a deadlock is *possible*, not certain. The processes may never actually claim
  their maxima.
- **Deadlocked** ⟹ already stuck; a strict subset of unsafe.

[[deadlock-avoidance]] is therefore deliberately **pessimistic**: it refuses requests that lead to
merely *unsafe* states, some of which would have been perfectly fine.

## Related

- [[bankers-algorithm]] — the procedure that keeps the state safe.
- [[deadlock-avoidance]] — the strategy.
- [[deadlock-detection-marking-algorithm]] — the test used.
- [[deadlock]] — the outcome being avoided.

## Common confusions

- **Unsafe does not mean deadlocked.** This is the single most-missed point. Unsafe means "no
  guaranteed completion order exists", not "stuck".
- **Safety is about the *maximum* claim, not the current request.** A process holding little but
  entitled to much can make a state unsafe.
- **The safe sequence need not be unique**, and the algorithm only has to find *one*.

## Sources

- [[lecture-bs-3]] (p21, p27)
