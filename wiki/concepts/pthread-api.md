---
title: pthread API
type: concept
tags: [thread, posix, c, api]
chapter: "02"
sources: [lecture-bs-2, lecture-bs-4]
created: 2026-09-30
updated: 2026-09-30
---

# pthread API

## Definition

The **pthread (POSIX threads) API** is the C interface used in the exercises to create, terminate
and wait for [[thread|threads]].

## Context

Belongs to [[threads]]. It is the practical counterpart to [[fork]]/[[exec]] for processes.

## How it works

| Call | Effect |
|---|---|
| `pthread_create()` | **starts a new thread in the calling process**; the supplied `start_routine` is called |
| `pthread_exit()` | terminates the calling thread |
| `pthread_join()` | waits for another thread to finish |

`pthread_create()` is the thread-level analogue of [[fork]], with one crucial difference: it takes
the function to run as an argument, so there is no separate [[exec]] step and no duplicated
address space.

### What happens if `pthread_join()` is removed?

**When the process terminates, the other thread is terminated immediately as well** — the created
thread never gets to finish. `pthread_join()` is therefore the thread-level counterpart of
`waitpid()`: without it the created thread is not waited for, and unlike a
[[orphan-process|orphaned process]] it is not adopted by anyone — it simply dies with the process.

### Building a thread-safe queue

The "Praxis 4" sheet assembles the full set into one object — a queue guarded by **one mutex and
two condition variables**:

```c
void queue_init(queue_t *queue) {
    /* ... */
    pthread_mutex_init(&queue->mutex,    NULL);
    pthread_cond_init (&queue->canWrite, NULL);   /* members of queue */
    pthread_cond_init (&queue->canRead,  NULL);
}

void queue_put(queue_t *queue, char b) {
    pthread_mutex_lock(&queue->mutex);
    while (queue->size == QueueMax) {
        pthread_cond_wait(&queue->canWrite, &queue->mutex);
    }
    /* FIFO insert */
    queue->size++;
    pthread_cond_signal(&queue->canRead);
    /* pthread_mutex_unlock(&queue->mutex); */
}
```

Three details the exercise highlights:

1. **`while`, not `if`.** `pthread_cond_wait` may return spuriously, and another thread may have
   consumed the slot in between — the predicate must be re-tested. See [[condition-variable]].
2. **`pthread_cond_wait` takes the mutex as its second argument.** It atomically releases the
   mutex while blocking and re-acquires it before returning; otherwise no other thread could ever
   change the predicate.
3. **Two condition variables, one mutex.** `canWrite` for a full queue and `canRead` for an empty
   one — signalling the wrong one would wake threads that still cannot proceed.

This is exactly the [[producer-consumer]] pattern.

## Related

- [[thread]] — the object being created.
- [[thread-state-model]] — the transitions these calls trigger.
- [[fork]] and [[exec]] — the process-level equivalents.
- [[signal-handler]] — the other C-level mechanism from the same exercise sheet.
- [[mutex]] — `pthread_mutex_init` / `_lock` / `_unlock` / `_destroy`, added in chapter 3.
- [[condition-variable]] — `pthread_cond_init` / `_wait` / `_signal`, added in chapter 3.
- [[producer-consumer]] — the canonical program written with these two.
- [[semaphore]] — the System V alternative (`semop`) used in the same practical sheet.

## Common confusions

- **`pthread_create` does not duplicate anything.** Unlike [[fork]], there is no copy of the
  address space and therefore no [[copy-on-write]] question at all.
- **Omitting `pthread_join()` is not a resource leak like a [[zombie-process]]** — the thread is
  simply killed with the process.
- The thread that runs `main()` and a thread created by `pthread_create()` differ only in **stack
  and PC**.

## Sources

- [[lecture-bs-2]] (p02, p08)
