---
title: Producer–Consumer (Bounded Buffer)
type: algorithm
tags: [concurrency, synchronisation, algorithm, producer-consumer, condition-variable]
chapter: "03"
sources: [lecture-bs-3, lecture-bs-4]
created: 2026-09-30
updated: 2026-09-30
---

# Producer–Consumer (Erzeuger–Verbraucher)

## Purpose

Let one or more **producers** insert items into a **bounded buffer** and one or more
**consumers** remove them, such that

1. no two strands corrupt the buffer ([[mutual-exclusion]]),
2. a producer **waits** when the buffer is **full**,
3. a consumer **waits** when the buffer is **empty**.

It is the canonical worked example of combining a [[mutex]] with
[[condition-variable|condition variables]], and the model for every OS queue,
[[pipe]] and I/O buffer.

## Idea

Two orthogonal problems, two different tools:

- **Protecting the buffer** is a mutual-exclusion problem → one [[mutex]].
- **Waiting for a state** ("not full", "not empty") is a predicate problem → two
  [[condition-variable|condition variables]] attached to that mutex.

Using the mutex alone would deadlock: a producer holding the lock and waiting for space prevents
any consumer from ever making space. The condition variable exists precisely to release the lock
while waiting — see [[condition-variable]].

## Procedure

The Java version from the notes, using `ReentrantLock` and two `Condition`s:

```java
class BoundedQueue<T> {
    private final Object[]  items = new Object[N];
    private int count = 0, head = 0, tail = 0;

    private final ReentrantLock lock  = new ReentrantLock();
    private final Condition     full  = lock.newCondition();   // "buffer is full"
    private final Condition     empty = lock.newCondition();   // "buffer is empty"

    public void put(T x) throws InterruptedException {
        lock.lock();
        try {
            while (count == items.length)   // while, NOT if
                full.await();               // releases the lock, blocks, re-acquires
            items[tail] = x;
            tail = (tail + 1) % items.length;
            count++;
            empty.signal();                 // a consumer may now proceed
        } finally { lock.unlock(); }
    }

    @SuppressWarnings("unchecked")
    public T take() throws InterruptedException {
        lock.lock();
        try {
            while (count == 0)              // while, NOT if
                empty.await();
            T x = (T) items[head];
            head = (head + 1) % items.length;
            count--;
            full.signal();                  // a producer may now proceed
            return x;
        } finally { lock.unlock(); }
    }
}
```

### The three things the exam asks about

1. **`while`, not `if`.** When a waiting thread is woken it has not yet been scheduled and does
   **not yet hold the mutex**. Before it runs again another thread may enter the
   [[critical-section]] and add or remove an element, so the predicate must be re-checked. Using
   `if` is simply **incorrect**.
2. **Why the condition variable is tied to the lock.** The waiter blocks *inside* the critical
   section; for its condition to become true another thread must enter that same section, so the
   lock must be released for the duration of the blocking and re-acquired afterwards.
3. **Two conditions, not one.** Signalling a single shared condition could wake a producer when
   space was never created, wasting a wakeup — and with `if` instead of `while` it would be a bug.

## Complexity / cost

- Each `put`/`take` costs one lock acquisition and, when the buffer is at a limit, a block and a
  wake — i.e. two [[process-switch|switches]].
- The buffer decouples producer and consumer speeds: the bound limits memory use and applies
  back-pressure.
- With a single lock, producers and consumers contend even though they touch different ends of
  the buffer.

## Related

- [[mutex]] — the lock.
- [[condition-variable]] — `full` and `empty`.
- [[semaphore]] — the classical alternative formulation with counting semaphores.
- [[critical-section]] — what the lock protects.
- [[monitor]] — the language-level packaging of the same structure.
- [[pipe]] — the OS-level instance of this pattern.
- [[deadlock]] — what happens if the lock is held while waiting.
- [[pthread-api]] — the "Praxis 4" sheet builds exactly this structure as a thread-safe queue with
  the condition variables `canWrite` and `canRead`.

## Sources

- [[lecture-bs-3]] (p12, p25)
- [[lecture-bs-4]] (p29) — the `queue_init` / `queue_put` implementation
