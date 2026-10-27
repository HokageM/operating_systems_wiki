---
title: Mutex
type: concept
tags: [concurrency, synchronisation, lock, mutex, pthreads]
chapter: "03"
sources: [lecture-bs-3]
created: 2026-09-30
updated: 2026-09-30
---

# Mutex

## Definition

A **mutex** (from *mutual exclusion*) is a synchronisation object used to protect a
[[critical-section]], which is always in exactly one of two states — **owned (locked)** or
**not owned (unlocked)** — and which may be **unlocked only by the [[thread]] that owns it**.

A thread that tries to lock an owned mutex is **blocked** (passive waiting), not spun.

## Context

Belongs to [[concurrency-and-synchronisation]]. The mutex is the blocking counterpart of the
[[spin-lock]] and the practical everyday implementation of the [[lock]] interface. It pairs with
a [[condition-variable]] to form the [[producer-consumer]] pattern, and the compiler-generated
version of it is a [[monitor]].

## How it works

```c
pthread_mutex_t m;
pthread_mutex_init(&m, NULL);

pthread_mutex_lock(&m);      /* blocks if owned by another thread */
   /* critical section */
pthread_mutex_unlock(&m);    /* only the owner may do this */

pthread_mutex_destroy(&m);
```

This is part of the [[pthread-api]]. Blocking puts the thread into **Blocked** in the
[[thread-state-model]] and off the ready queue, so it consumes no CPU — at the price of **two
[[process-switch|switches]]**, one to leave and one to return.

### Mutex versus binary semaphore

This comparison is asked directly and is the most exam-relevant part of the page.

| | **Mutex** | **Binary [[semaphore]]** |
|---|---|---|
| **Ownership** | yes — it belongs to the locking thread | no — it belongs to nobody |
| **Who may release** | **only the owner** | **any** process |
| **Purpose** | [[mutual-exclusion]] only | mutual exclusion **and** signalling between processes |
| **Nature** | a lock | a counter with a queue |

Because a semaphore can be released by a *different* process, it can be used to signal an event;
a mutex cannot. Conversely, the ownership rule makes a mutex safer and makes errors detectable.

## Related

- [[lock]] — the interface it implements.
- [[spin-lock]] — the busy-waiting alternative.
- [[semaphore]] — the more general primitive.
- [[condition-variable]] — what a mutex is combined with to wait for predicates.
- [[monitor]] — the language-level packaging.
- [[pthread-api]] — the concrete calls.
- [[deadlock]] — what two mutexes taken in opposite orders produce.

## Common confusions

- **"Mutex = binary semaphore" is wrong**, and the notes single this out: the difference is
  **ownership**. A binary semaphore released by a foreign process is legal; a mutex unlocked by a
  non-owner is an error.
- **Blocking is not free.** The reason [[spin-lock|spin locks]] still exist is that a mutex costs
  two switches even when the wait would have been microseconds.
- **A mutex does not protect anything by itself.** It protects only the sections that agree to
  take it.
- **Locking two mutexes in inconsistent order is the textbook [[deadlock]]** — hold-and-wait plus
  circular wait, both present.

## Sources

- [[lecture-bs-3]] (p06, p11, p12, p23)
