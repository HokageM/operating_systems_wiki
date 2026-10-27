---
title: Dining Philosophers
type: algorithm
tags: [deadlock, livelock, starvation, semaphore, algorithm]
chapter: "03"
sources: [lecture-bs-3]
created: 2026-09-30
updated: 2026-09-30
---

# Dining Philosophers (Philosophenproblem)

## Purpose

The standard illustration of every failure mode in [[deadlocks]] at once. Five philosophers sit
around a table; between each pair lies **one fork**, five in total. A philosopher alternates
between thinking and eating, and **needs both neighbouring forks to eat**.

The problem is to write `take_forks` / `put_forks` so that the philosophers never
[[deadlock]], never [[livelock]] and — ideally — never [[starvation|starve]].

## Idea

The naive solution fails, and each fix reveals a new failure:

| Attempt | Failure |
|---|---|
| Each philosopher takes the **left** fork, then the **right** | **[[deadlock]]**: if all five take the left fork simultaneously, each waits forever for a right fork that will never be released. The [[resource-allocation-graph]] is a five-cycle. |
| Take left; if right is taken, **put left down, wait, retry** | **[[livelock]]**: all five do the same thing at the same time and loop forever without eating. Active waiting, no progress. |
| Guard the whole state with one lock and only eat when **both** neighbours are not eating | works — no deadlock, no livelock — but **[[starvation]] remains possible**: a philosopher whose two neighbours eat in alternation never gets a turn. |

The third is the solution the notes present.

## Procedure

Using one binary [[semaphore]] `lock` to protect the state array, plus one semaphore per
philosopher to block them individually:

```c
#define N 5
#define LEFT  ((i + N - 1) % N)
#define RIGHT ((i + 1) % N)

typedef enum { THINKING, HUNGRY, EATING } state_t;

state_t   state[N];
semaphore lock = 1;      /* binary semaphore: protects state[] */
semaphore s[N];          /* all initialised to 0: one per philosopher */

void take_forks(int i) {
    acquire(&lock);
        state[i] = HUNGRY;
        check_forks(i);          /* may set state[i] = EATING and release(&s[i]) */
    release(&lock);
    acquire(&s[i]);              /* blocks unless check_forks already released it */
}

void put_forks(int i) {
    acquire(&lock);
        state[i] = THINKING;
        check_forks(LEFT);       /* a neighbour may now be able to eat */
        check_forks(RIGHT);
    release(&lock);
}

void check_forks(int i) {
    if (state[i] == HUNGRY
        && state[LEFT]  != EATING
        && state[RIGHT] != EATING) {
        state[i] = EATING;
        release(&s[i]);          /* wake philosopher i */
    }
}
```

### Why it works

- **No deadlock.** `state[]` is only ever changed inside the `lock` [[critical-section]], and a
  philosopher becomes `EATING` only when **both** neighbours are not eating — so both forks are
  atomically acquired together. Hold-and-wait is eliminated: a philosopher never holds one fork
  while waiting for the other.
- **No livelock.** A blocked philosopher waits **passively** on `s[i]` and is woken explicitly by
  a departing neighbour's `check_forks`. There is no retry loop.
- **Starvation is still possible.** Nothing orders the hungry philosophers, so if the two
  neighbours of philosopher *i* keep alternating, `check_forks(i)` never finds both idle. Fixing
  this needs a fairness mechanism — a FIFO queue, as in a strong [[semaphore]] or a
  [[ticket-spin-lock]].

### The simpler alternative

**Linear resource ordering** ([[deadlock-prevention]], condition 4): number the forks and require
each philosopher to take the **lower-numbered fork first**. One philosopher then takes the right
fork first, the cycle is broken, and no deadlock is possible — with far less machinery.

## Complexity / cost

- N + 1 semaphores; each meal costs two short critical sections.
- Maximum concurrency is ⌊N/2⌋ = 2 philosophers eating at once for N = 5.
- The single `lock` serialises all state changes — fine at this scale, a bottleneck at large N.

## Related

- [[deadlock]] — the naive failure.
- [[livelock]] — the failure of the symmetric fix.
- [[starvation]] — what survives the correct solution.
- [[semaphore]] — the primitive used.
- [[resource-allocation-graph]] — the five-cycle.
- [[deadlock-prevention]] — linear ordering as the cheaper fix.
- [[critical-section]] — the `lock`-protected region.

## Sources

- [[lecture-bs-3]] (p15, p16)
