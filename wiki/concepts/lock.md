---
title: Lock
type: concept
tags: [concurrency, synchronisation, lock]
chapter: "03"
sources: [lecture-bs-3]
created: 2026-09-30
updated: 2026-09-30
---

# Lock (Sperre)

## Definition

A **lock** is the abstract two-operation interface used to enforce [[mutual-exclusion]]:

```java
interface Lock {
    void lock();     // enter the critical section, wait if necessary
    void unlock();   // leave the critical section
}
```

Every concrete mechanism in this chapter — [[petersons-algorithm]], [[test-and-set]],
[[spin-lock]], [[ticket-spin-lock]], [[mutex]], binary [[semaphore]] — implements this same
interface. Only the *implementation* of `lock()` differs.

## Context

Belongs to [[concurrency-and-synchronisation]]. The lock is what brackets a
[[critical-section]]; the interface is deliberately minimal so that implementations can be
swapped without touching the protected code.

## How it works

`lock()` must not return until the caller is the sole occupant of the critical section. The two
families of implementation differ in what the caller does while waiting:

| | Busy waiting | Blocking |
|---|---|---|
| Implementation | [[spin-lock]], [[ticket-spin-lock]] | [[mutex]], [[semaphore]] |
| State while waiting | **Running** — loops on the lock variable | **Blocked** — queued, off the CPU |
| Cost | wasted CPU cycles | two [[process-switch\|process switches]] |
| Best when | expected wait **<** a [[context-switch]] | expected wait is long |

The chapter's failed attempts are instructive as implementations of this same interface:

- A plain **lock variable** (Sperrvariable) — `while (locked); locked = true;` — fails because
  the test and the set are not an [[atomic-operation]]. Two processes can both read `false`.
- **Strict alternation** (strikte Alternation) — `while (turn != me);` … `turn = other;` — is
  correct but violates the **progress** requirement: a process that stays in its remainder
  section forever never hands `turn` back.

## Related

- [[mutual-exclusion]] — the property `lock()` delivers.
- [[critical-section]] — what it brackets.
- [[mutex]] — a lock with an owner.
- [[semaphore]] — a generalisation with a counter.
- [[condition-variable]] — always attached to a lock.
- [[monitor]] — a lock the compiler inserts for you.

## Common confusions

- **`lock()`/`unlock()` are not symmetric in cost.** `unlock()` is cheap and never waits;
  `lock()` is where all the design lives.
- **The lock protects a *convention*, not the data.** Nothing stops code from touching the shared
  variable without locking; correctness depends on every accessor agreeing to use the lock.
- **A correct lock is not automatically a fair lock.** See [[spin-lock]] versus
  [[ticket-spin-lock]].

## Sources

- [[lecture-bs-3]] (p03)
