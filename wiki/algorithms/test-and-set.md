---
title: Test-and-Set
type: algorithm
tags: [concurrency, mutual-exclusion, algorithm, hardware]
chapter: "03"
sources: [lecture-bs-3]
created: 2026-09-30
updated: 2026-09-30
---

# Test-and-Set (TSL)

## Purpose

Provide [[mutual-exclusion]] for **any number of processes** with **hardware support**: a single
machine instruction that **reads a memory word and writes a new value to it
[[atomic-operation|atomically]]**, so that no other process can interleave between the read and
the write.

This is precisely what the naive lock variable lacks — see [[lock]].

## Idea

The whole problem with

```c
while (locked) ;      /* test  */
locked = true;        /* set   */
```

is the gap between the two lines: two processes can both read `false` and both proceed. A
test-and-set instruction closes the gap by performing both in one indivisible step, enforced by
the CPU (on a multiprocessor, by locking the memory bus or the cache line).

## Procedure

```c
/* executed atomically by the hardware */
int test_and_set(int *lock) {
    int old = *lock;
    *lock = 1;
    return old;
}

void lock(int *l)   { while (test_and_set(l) == 1) ; }   /* spin */
void unlock(int *l) { *l = 0; }
```

- If the previous value was **0**, the lock was free and this call has just taken it → enter.
- If the previous value was **1**, the lock was already taken and writing 1 again changes nothing
  → spin and retry.

This packaging is exactly the [[spin-lock]].

## Complexity / cost

- **Correct for n processes**, unlike [[petersons-algorithm]].
- **Requires hardware support** — the instruction must exist (`TSL`, `XCHG`, `CMPXCHG`, `LDREX/STREX`).
- **Busy waiting**: the waiter stays Running. Worth it only when the expected wait is shorter than
  a [[context-switch]].
- **Not fair**: the winner of each retry race is arbitrary, so [[starvation]] is possible. The
  [[ticket-spin-lock]] fixes this.
- On a multiprocessor, many spinners hammering the same cache line cause heavy bus traffic.

## Related

- [[atomic-operation]] — the property the hardware supplies.
- [[spin-lock]] — the lock built directly on it.
- [[ticket-spin-lock]] — the fair version.
- [[petersons-algorithm]] — the software-only alternative.
- [[mutex]], [[semaphore]] — the blocking primitives, themselves implemented on top of this inside
  the [[kernel]].
- [[lock]] — the failed non-atomic attempt this replaces.

## Sources

- [[lecture-bs-3]] (p05)
