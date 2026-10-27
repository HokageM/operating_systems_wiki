---
title: Race Condition
type: concept
tags: [concurrency, synchronisation, race-condition, bug]
chapter: "03"
sources: [lecture-bs-3]
created: 2026-09-30
updated: 2026-09-30
---

# Race Condition (zeitkritischer Ablauf)

## Definition

A **race condition** (zeitkritischer Ablauf) is a situation in which **the result of a
computation depends on the temporal order in which concurrent processes access a shared
[[resource]]**.

Verbatim from the notes, and asked directly in the exercise recap (3-1 b).

## Context

Belongs to [[concurrency-and-synchronisation]]. It is the harmful subset of a [[conflict]] and
the reason [[mutual-exclusion]] is needed at all. The German name is the more descriptive one:
*zeitkritisch* — "timing-critical" — says exactly what the defect is.

## How it works

The chapter's framing assumption is that **no assumption may be made about the temporal course of
execution**. The [[scheduler]] may preempt a [[thread]] between any two machine instructions, and
on a multiprocessor two threads genuinely run at the same instant.

A single `x = x + 1` is already three steps:

```
load  r, x
add   r, 1
store x, r
```

If two threads interleave as *load, load, add, add, store, store*, one increment disappears. The
program is not wrong on any single path — it is wrong on *some* paths, which is what makes race
conditions so hard to find.

Properties that follow:

- **Non-deterministic.** The same input can give different outputs on different runs.
- **Not reproducible.** Adding a debug print changes the timing and often hides the bug.
- **Not fixable by testing.** Absence of failure in a thousand runs proves nothing.

The fix is to make the [[critical-section]] behave as an
[[atomic-operation|atomic instruction sequence]] via [[mutual-exclusion]].

## Related

- [[conflict]] — the broader precondition.
- [[critical-section]] — where the race happens.
- [[atomic-operation]] — the property that removes it.
- [[mutual-exclusion]] — the mechanism that supplies atomicity.
- [[lock]], [[mutex]], [[semaphore]] — concrete tools.

## Common confusions

- **The bug is not in the shared variable, it is in the access pattern.** `volatile` alone does
  not fix a race condition — it only forbids caching the value in a register; it does not make
  read-modify-write atomic. This is why [[petersons-algorithm]] needs *both* `volatile` and the
  algorithm itself.
- **A race condition is not a [[deadlock]].** A race produces a *wrong* result quickly; a deadlock
  produces *no* result at all.
- **Multiprocessors are not required.** Preemption on a single CPU is enough.

## Sources

- [[lecture-bs-3]] (p01, p22)
