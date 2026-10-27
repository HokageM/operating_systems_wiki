---
title: Condition Variable
type: concept
tags: [concurrency, synchronisation, condition-variable, pthreads]
chapter: "03"
sources: [lecture-bs-3]
created: 2026-09-30
updated: 2026-09-30
---

# Condition Variable (Bedingungsvariable)

## Definition

A **condition variable** is a synchronisation object on which a [[thread]] can **wait until a
certain condition becomes true**, and which another thread can **signal** when it has made that
condition true. It is **always associated with a [[mutex]]**.

## Context

Belongs to [[concurrency-and-synchronisation]]. A [[mutex]] answers *"may I touch the data?"*; a
condition variable answers *"is the data in the state I need?"*. The two together make the
[[producer-consumer]] buffer possible, and a [[monitor]] bundles both.

## How it works

```c
pthread_cond_t  c;
pthread_mutex_t m;
pthread_cond_init(&c, NULL);

/* waiter */
pthread_mutex_lock(&m);
while (!condition)              /* while, NOT if */
    pthread_cond_wait(&c, &m);  /* releases m, blocks, re-acquires m on return */
/* ... */
pthread_mutex_unlock(&m);

/* signaller */
pthread_mutex_lock(&m);
make_condition_true();
pthread_cond_signal(&c);
pthread_mutex_unlock(&m);
```

Part of the [[pthread-api]]. The Java equivalent (`Condition`, `await()`, `signal()`) documents
the contract in one sentence, quoted verbatim in the notes:

> When the Condition await methods are called the lock is released and before they return, the
> lock is reacquired and the lock hold count restored to what it was before.

### Why a condition variable must be tied to a lock

This is asked explicitly (exercise 3-3 a) and the reasoning is the point of the whole concept:

1. The waiting thread blocks **inside the [[critical-section]]**.
2. For its wait condition ever to become true, **another thread must enter that same critical
   section**.
3. Therefore the lock **must be released for the duration of the blocking** — otherwise nobody
   could ever make the condition true, and the program would [[deadlock]].
4. After the blocking ends, the lock **must be re-acquired** before the thread continues, because
   it is about to touch the shared data again.

Releasing-and-blocking must happen atomically, which is why it cannot be written by the
programmer as `unlock(); block(); lock();` — a signal arriving in between would be lost.

### Why `while` and not `if`

Also asked directly (exercise 3-3 b). Using `if` is **not correct**:

- When a thread is woken it has not yet been scheduled and **does not yet hold the mutex**.
- Between the signal and the thread actually re-acquiring the mutex, **another thread can enter
  the critical section** and add or remove an element again.
- By the time the woken thread runs, the condition it was waiting for may be false again.

So the predicate must be **re-checked in a loop**. (The same loop also absorbs spurious wakeups.)

## Related

- [[mutex]] — the lock it is always paired with.
- [[monitor]] — condition variables plus implicit mutual exclusion.
- [[producer-consumer]] — the canonical use: `full` and `empty` conditions.
- [[semaphore]] — an alternative that carries a count instead of a predicate.
- [[pthread-api]] — `pthread_cond_init`, `pthread_cond_wait`, `pthread_cond_signal`.
- [[thread-state-model]] — `wait()` moves the thread to Blocked.

## Common confusions

- **A condition variable holds no state.** A `signal()` with no waiter is simply lost. The
  predicate lives in *your* shared variables, which is exactly why the `while` loop is needed.
- **`signal()` does not transfer the mutex.** It marks a thread runnable; the thread must still
  compete for the mutex like everyone else.
- **It is not a [[semaphore]].** A semaphore's `release()` increments a counter that persists; a
  condition's `signal()` does not.

## Sources

- [[lecture-bs-3]] (p07, p12, p25)
