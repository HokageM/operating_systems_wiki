---
title: Semaphore
type: concept
tags: [concurrency, synchronisation, semaphore]
chapter: "03"
sources: [lecture-bs-3, lecture-bs-4]
created: 2026-09-30
updated: 2026-09-30
---

# Semaphore (Semaphor)

## Definition

A **semaphore** is a synchronisation primitive consisting of an **integer counter** and a
**queue of blocked processes**, with two **atomic** operations:

- **`acquire()`** (P, wait, `down`) — decrement the counter; if it becomes negative, block the
  caller and put it into the queue.
- **`release()`** (V, signal, `up`) — increment the counter; if processes are waiting, wake one.

Both operations may be called by **different processes**, and both must be
[[atomic-operation|atomic]] — which is why they are realised as [[system-call|system calls]].

## Context

Belongs to [[concurrency-and-synchronisation]]. The semaphore is the most general primitive in
the chapter: it subsumes the [[mutex]] (as a binary semaphore) and adds resource counting and
cross-process signalling. It is the tool used in the [[dining-philosophers]] solution.

## How it works

### As a data structure

```c
struct Semaphore {
    int   count;
    Queue queue;    /* processes blocked on this semaphore */
};
```

### Meaning of the counter

| `count` | Meaning |
|---|---|
| **> 0** | that many units of the resource are still free; `acquire()` succeeds immediately |
| **= 0** | no unit is free, but nobody is waiting yet |
| **< 0** | the absolute value gives the number of processes blocked in the queue |

### Binary versus counting

- A **binary semaphore** (binäres Semaphor) is initialised to 1 and only ever holds 0 or 1. It
  implements [[mutual-exclusion]] over one [[critical-section]].
- A **counting semaphore** (zählendes Semaphor) is initialised to *n* and manages **n
  interchangeable units** of a [[resource]] — buffer slots, free connections, identical devices.

### Strong versus weak

- A **strong semaphore** (starkes Semaphor) wakes processes in **FIFO** order. This guarantees
  fairness and excludes [[starvation]].
- A **weak semaphore** (schwaches Semaphor) makes **no ordering guarantee** — any waiting process
  may be chosen, so starvation is possible.

### Semaphore versus mutex

| | [[semaphore]] | [[mutex]] |
|---|---|---|
| Ownership | **none** | the locking thread owns it |
| Who may release | **any** process | **only the owner** |
| State | integer counter + queue | locked / unlocked |
| Uses | mutual exclusion, resource counting, **signalling between processes** | mutual exclusion only |

"A semaphore is a pure counter" is the notes' one-line summary of the difference.

### System V semaphores in the exercises

The practical sheets use the **System V** semaphore interface rather than POSIX semaphores:

| | |
|---|---|
| `ipcs -s` | shell command; **shows the existing semaphores** on the system |
| `struct sembuf` | the **buffer for semaphore operations** — one entry describes one operation |
| `int semop(int semid, struct sembuf *sops, size_t nsops)` | **carries out the actual operation** on the semaphore. `sops` is an **array of operations**, `nsops` is the **number of operations** |

The signature is worth reading closely: `semop` takes an *array*, so several operations can be
submitted in one call and are then performed **together and atomically**. That is exactly what is
needed to acquire two semaphores without risking a [[deadlock]] — the standard fix for the
[[dining-philosophers]] problem.

## Related

- [[mutex]] — the owned, mutual-exclusion-only relative.
- [[condition-variable]] — waits on a predicate rather than a count.
- [[monitor]] — the compiler-level alternative.
- [[dining-philosophers]] — uses one binary semaphore plus one semaphore per philosopher.
- [[producer-consumer]] — classically solved with counting semaphores.
- [[deadlock]] — a forgotten `release()` produces one.
- [[system-call]] — how the atomicity is obtained.
- [[pthread-api]] — the thread-level alternative used in the same practical sheets.

## Common confusions

- **A semaphore is not a lock with a number.** The absence of ownership is the defining
  difference, and it is what makes signalling possible: producer `release()`s, consumer
  `acquire()`s, and they are different processes.
- **`count < 0` is normal, not an error.** The magnitude is the number of blocked processes.
- **`acquire()` does not always block** — only when the counter goes negative.
- **Binary semaphore ≠ mutex.** See the table; this exact distinction is an exam question.
- **Semaphores do not prevent deadlock; they cause it.** Two semaphores acquired in opposite
  orders is the standard example.

## Sources

- [[lecture-bs-3]] (p08, p09, p10, p11, p16, p23)
- [[lecture-bs-4]] (p29) — the System V interface from the "Praxis 4" sheet
