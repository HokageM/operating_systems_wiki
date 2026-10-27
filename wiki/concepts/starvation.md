---
title: Starvation
type: concept
tags: [deadlock, starvation, fairness, scheduling]
chapter: "03"
sources: [lecture-bs-3, lecture-bs-5]
created: 2026-09-30
updated: 2026-09-30
---

# Starvation (Verhungern)

## Definition

**Starvation** is the situation in which a process **waits indefinitely for a [[resource]]
although the resource repeatedly becomes available**, because it is always passed over in favour
of other processes.

## Context

Belongs to [[deadlocks]], but it is equally a synchronisation and a scheduling concern: freedom
from starvation is the **fourth requirement** every [[mutual-exclusion]] procedure must satisfy.

## How it works

Starvation differs from [[deadlock]] and [[livelock]] in that **nothing is structurally broken**
— the system as a whole makes progress, one particular process simply never wins.

| | [[deadlock]] | [[livelock]] | **Starvation** |
|---|---|---|---|
| Scope | a **set** of processes | a **set** of processes | a **single** process |
| System progress | none | none | normal |
| Cause | circular wait | symmetric retreat | unfair arbitration |
| Resolvable by waiting | never | in principle | in principle |

Typical sources in this chapter:

- A plain [[spin-lock]]: whoever happens to test the variable first wins, so an unlucky process
  may lose every race.
- A **weak** [[semaphore]], which makes no ordering promise about which waiter is woken.
- The [[dining-philosophers]] solution with per-philosopher semaphores: it is deadlock-free and
  livelock-free, but a philosopher whose neighbours eat in alternation can still starve.

The fix is always **fairness through ordering**: FIFO queues. A **strong** [[semaphore]] and the
[[ticket-spin-lock]] both exclude starvation exactly this way.

## Starvation in CPU scheduling

Chapter 05 supplies the second family of examples, where starvation is caused not by an unfair lock
but by an unfair [[scheduling|scheduling policy]]:

| Strategy | Starves? | Who |
|---|---|---|
| [[first-come-first-served]] | **no** | the FIFO [[ready-queue]] is an ordering guarantee |
| [[shortest-process-next]] | **yes** | **long** processes, if short ones keep arriving |
| [[priority-scheduling]] (static) | **yes** | **low-priority** processes |
| [[round-robin]] | **no** | every process gets 1/n of the CPU and waits at most (n−1)·q |

The cure named in the notes is **[[ageing]]**: increase a process's priority with its waiting time,
so that no process can be passed over indefinitely. That is the same principle as the FIFO fix —
turn "who is most deserving" into something that eventually favours the longest waiter.

Note the pattern across both families: **the starvation-free strategies are exactly the ones with
an ordering guarantee** ([[first-come-first-served]], [[round-robin]], strong semaphores,
[[ticket-spin-lock]]), and the starving ones are exactly the ones that choose on merit.

## Related

- [[mutual-exclusion]] — requirement 4 is freedom from starvation.
- [[ticket-spin-lock]] — the FIFO fix for spin locks.
- [[semaphore]] — strong (FIFO, starvation-free) versus weak.
- [[dining-philosophers]] — starvation surviving a deadlock-free solution.
- [[scheduler]] — the other place starvation is decided.
- [[ageing]] — the scheduling cure.
- [[shortest-process-next]], [[priority-scheduling]] — the two strategies that starve processes.
- [[deadlock]], [[livelock]] — the two neighbours it is contrasted with.

## Common confusions

- **Starvation is not a deadlock.** No cycle exists and the resource *is* being granted — just
  never to this process. Solving deadlock does not solve starvation, as
  [[dining-philosophers]] demonstrates.
- **Fairness and correctness are independent.** A completely correct lock can starve processes
  forever; that is why the four requirements list them separately.
- **Randomness weakens but does not remove starvation** — only an ordering guarantee does.

## Sources

- [[lecture-bs-3]] (p02, p05, p14, p16)
- [[lecture-bs-5]] (p02, p05) — starvation under SPN and static priorities; ageing as the remedy.
