---
title: Monitor
type: concept
tags: [concurrency, synchronisation, monitor, language-support]
chapter: "03"
sources: [lecture-bs-3]
created: 2026-09-30
updated: 2026-09-30
---

# Monitor

## Definition

A **monitor** is a **programming-language construct** that bundles shared data together with the
procedures operating on it, and in which the **compiler automatically generates the
[[mutual-exclusion]]** — at most one process can be active inside the monitor at any time,
without the programmer writing a single [[lock]] call.

## Context

Belongs to [[concurrency-and-synchronisation]]. The monitor is the highest-level primitive in the
chapter: where a [[mutex]] must be taken and released explicitly, a monitor makes mutual exclusion
a property of the *type*. Java's `synchronized` is the familiar instance.

## How it works

- The programmer declares the data and the procedures; the **compiler inserts the lock and unlock**
  at procedure entry and exit.
- Because the mutual exclusion is generated rather than written, the classic errors — forgetting
  to unlock, unlocking on an early `return`, unlocking on an exception — become impossible.
- Waiting for a *predicate* inside a monitor still needs [[condition-variable|condition
  variables]], which is why Java pairs `synchronized` with `wait()` / `notify()`.

The chapter presents the monitor immediately before the [[semaphore]] as the contrast between
**language-level** and **OS-level** synchronisation:

| | [[monitor]] | [[semaphore]] |
|---|---|---|
| Provided by | the **compiler / language** | the **[[kernel]]**, as a [[system-call]] |
| Programmer writes | nothing | explicit `acquire()` / `release()` |
| Scope | within one program | across processes |
| Error-prone | hard to misuse | easy to misuse |

## Related

- [[mutex]] — what the compiler inserts underneath.
- [[condition-variable]] — still needed for predicate waiting.
- [[semaphore]] — the OS-level counterpart.
- [[mutual-exclusion]] — the property it automates.
- [[producer-consumer]] — expressible as a monitor.

## Common confusions

- **A monitor is not a kind of lock — it is a kind of module.** The lock is an implementation
  detail the compiler supplies.
- **Monitors do not prevent [[deadlock]].** Two monitors entered in opposite orders deadlock just
  like two mutexes; automation removes the *forgetting* errors, not the *ordering* errors.
- **Monitors do not work across processes**, because there is no shared compiler-managed object —
  that is what [[semaphore|semaphores]] are for.

## Sources

- [[lecture-bs-3]] (p08)
