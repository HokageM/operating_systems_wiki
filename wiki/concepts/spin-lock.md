---
title: Spin Lock
type: concept
tags: [concurrency, synchronisation, lock, busy-waiting]
chapter: "03"
sources: [lecture-bs-3]
created: 2026-09-30
updated: 2026-09-30
---

# Spin Lock

## Definition

A **spin lock** is a [[lock]] in which a process that cannot enter the [[critical-section]]
**waits actively** — it loops, continuously re-testing the lock variable, and stays in the
**Running** state the whole time.

The waiting is called **active waiting** or **busy waiting** (aktives Warten, geschäftiges
Warten).

## Context

Belongs to [[concurrency-and-synchronisation]]. The spin lock is the packaging of
[[test-and-set]] into a usable primitive, and the direct counterpart of the blocking
[[mutex]].

## How it works

```c
void lock(int *l)   { while (test_and_set(l) == 1) /* spin */ ; }
void unlock(int *l) { *l = 0; }
```

The atomicity of [[test-and-set]] is what makes this correct — see [[lock]] for why the
non-atomic version fails.

### Advantages and disadvantages

| Advantage | Disadvantage |
|---|---|
| **No [[context-switch]]** — the process keeps the CPU and enters the section the instant it becomes free | **Wastes CPU time**: the waiting process does nothing useful for the whole wait |
| Very simple and small | **[[starvation]] is possible**: nothing guarantees which spinner wins the next race |
| Worth it when the expected wait is **shorter than a context switch** | Catastrophic on a single CPU if the lock holder is preempted — the spinner burns its entire time slice waiting for a process that cannot run |

The chapter states the decision rule explicitly: **a spin lock is advantageous exactly when the
expected waiting time is less than the cost of a [[context-switch]]**, because blocking costs
**two** process switches (out and back in). This is the standard exam answer.

The fairness problem is fixed by the [[ticket-spin-lock]], which serves waiters in FIFO order.

## Related

- [[test-and-set]] — the atomic instruction it is built on.
- [[ticket-spin-lock]] — the fair variant.
- [[mutex]] — the blocking alternative.
- [[context-switch]] — the cost the trade-off is measured against.
- [[starvation]] — its characteristic failure.
- [[livelock]] — spinning is active waiting, which is also what distinguishes a livelock from a
  [[deadlock]].

## Common confusions

- **Busy waiting is not the same as a livelock.** A spinning process *will* proceed once the lock
  is freed; a [[livelock]] never proceeds at all. Both are "active waiting", which is why the
  distinction is worth stating.
- **Spin locks are not slow — they are wasteful.** They have the *lowest* latency of any lock;
  what they waste is the CPU time of the waiter.
- **On a uniprocessor a spin lock is almost always wrong**, because the holder cannot run while
  the spinner spins. Spin locks belong in multiprocessor kernels.

## Sources

- [[lecture-bs-3]] (p05, p06, p23)
