---
title: Concurrency and Synchronisation
type: topic
tags: [concurrency, synchronisation, mutual-exclusion, lock, semaphore]
chapter: "03"
sources: [lecture-bs-3]
created: 2026-09-30
updated: 2026-09-30
---

# Concurrency and Synchronisation (Nebenläufigkeit und Synchronisation)

## Summary

[[thread|Threads]] share a data segment and [[process|processes]] can share memory, so two
strands of execution may touch the same datum at the same time. That is a
[[conflict|conflict]], and if the result depends on the interleaving it is a
[[race-condition|race condition]] (zeitkritischer Ablauf). The cure is
[[mutual-exclusion]]: at most one strand may be inside the [[critical-section]] at a time, so
that the section behaves as an [[atomic-operation|atomic instruction sequence]].

Every solution must satisfy four requirements — correctness, progress, fairness and freedom from
starvation. The chapter walks up a ladder of implementations: disabling
[[interrupt|interrupts]], simple lock variables and strict alternation (broken),
[[petersons-algorithm|Peterson's algorithm]] (correct, software only),
[[test-and-set]] (correct, with hardware support), the [[spin-lock]] and its fair
[[ticket-spin-lock|ticket]] variant, and then the blocking primitives the OS actually offers:
[[mutex]], [[condition-variable]], [[monitor]] and [[semaphore]]. The
[[producer-consumer]] bounded buffer is the canonical worked example.

The sequel is [[deadlocks]] — the price of blocking.

## Why it exists

Concurrency is wanted: it hides I/O latency, uses multiple cores, and keeps programs responsive.
But the notes open with the consequence — **no assumption may be made about the temporal course
of execution**. The scheduler may preempt anywhere, including between the read and the write of
`x = x + 1`. Without a rule for shared data, concurrent programs are correct only by luck, and
the bugs are non-deterministic and unreproducible. Synchronisation supplies that rule.

## Key concepts

- [[conflict]] — two strands access the same resource
- [[race-condition]] — the result depends on the timing
- [[mutual-exclusion]] — the remedy, plus its four requirements
- [[critical-section]] — the code region being protected
- [[atomic-operation]] — the property a critical section must simulate
- [[lock]] — the `lock()` / `unlock()` interface
- [[spin-lock]] — busy waiting
- [[mutex]] — a blocking, *owned* lock
- [[condition-variable]] — waiting for a predicate, not for a lock
- [[monitor]] — compiler-generated mutual exclusion
- [[semaphore]] — a counting synchronisation primitive

## Mechanisms / algorithms

### The four requirements for a mutual-exclusion procedure

| Requirement | Meaning |
|---|---|
| **Correctness** (Korrektheit) | At most one process is in the critical section at any time. |
| **Progress** (Fortschrittsbedingung) | A process outside the critical section must not block another process from entering. |
| **Fairness** (Fairness) | Every process gets a chance to enter — no unbounded overtaking. |
| **No starvation** (kein Verhungern) | No process waits forever. |

These four are the standard grading rubric for every proposed solution, and in the exam each
proposal is evaluated against them one by one.

### The ladder of solutions

| Solution | Correct? | Problem |
|---|---|---|
| Disable [[interrupt\|interrupts]] | yes, on 1 CPU | kernel-only, kills the scheduler, useless on multiprocessors |
| Simple lock variable | **no** | the test and the set are not atomic — a [[race-condition]] in the lock itself |
| Strict alternation (`turn`) | yes | **violates progress** — a process outside the section blocks the other |
| [[petersons-algorithm]] | yes | software only, but busy waiting; does not scale past 2 |
| [[test-and-set]] | yes | needs hardware support; busy waiting |
| [[ticket-spin-lock]] | yes | fair (FIFO), still busy waiting |
| [[mutex]] / [[semaphore]] | yes | blocking — costs two process switches |

### Busy waiting versus blocking

- A [[spin-lock]] burns CPU in a loop but keeps the process on the CPU. It **pays off when the
  expected wait is shorter than a [[context-switch]]**.
- Blocking ([[mutex]], [[semaphore]]) gives the CPU away but costs **two process switches** — out
  and back in again.
- This is the single trade-off that decides which primitive to use.

### Blocking primitives at a glance

| | Owner? | Counter? | Purpose |
|---|---|---|---|
| [[mutex]] | yes — only the owner may unlock | no (locked / unlocked) | mutual exclusion only |
| [[semaphore]] | no — any process may release | yes (`count` + queue) | mutual exclusion *and* signalling / resource counting |
| [[condition-variable]] | tied to a mutex | no | wait until a predicate holds |
| [[monitor]] | implicit, compiler-generated | no | mutual exclusion without explicit calls |

## Trade-offs

- **Spinning versus blocking** is a latency bet: short waits favour spinning, long waits favour
  blocking. Getting it wrong wastes either CPU cycles or switch overhead.
- **Correctness versus progress.** Strict alternation is perfectly correct and completely
  unusable — a reminder that correctness alone is not the goal.
- **Fairness costs bookkeeping.** The plain [[spin-lock]] is small and fast but starvation-prone;
  the [[ticket-spin-lock]] adds a FIFO queue to fix it.
- **Semaphores are powerful and error-prone.** They can signal across processes, but nothing
  enforces pairing — a missing `release()` is a silent [[deadlock]].
- **Coarse locking is easy, fine locking is fast.** Enlarging the critical section trivially
  restores correctness and destroys the concurrency you wanted.

## Exam relevance

- *"Was ist ein Konflikt / ein zeitkritischer Ablauf / wechselseitiger Ausschluss?"* — three
  verbatim definitions: [[conflict]], [[race-condition]], [[mutual-exclusion]]. Asked in both the
  lecture part and the exercise recap of this chapter.
- *"Welche vier Anforderungen muss ein Verfahren zum wechselseitigen Ausschluss erfüllen?"* — the
  table above. Very likely.
- *"Was ist ein kritischer Abschnitt / eine atomare Anweisungsfolge?"* — [[critical-section]],
  [[atomic-operation]].
- *"Vor- und Nachteile eines Spin Locks?"* — [[spin-lock]]; the answer hinges on the comparison
  with the [[context-switch]] cost.
- *"Unterschied Mutex und (binäres) Semaphor?"* — ownership; see [[mutex]] and [[semaphore]].
- *"Warum werden Condition Variables benötigt und warum sind sie mit einem Lock verknüpft?"* —
  [[condition-variable]]; the blocked thread must release the section so another can make the
  condition true.
- *"Warum `while` und nicht `if` um `await()`?"* — the woken thread does not yet hold the mutex.
- Write or complete [[petersons-algorithm]], or the [[producer-consumer]] buffer.

## Open questions

- The notes do not discuss **barriers**, **reader/writer locks** or **lock-free** programming.
- **Priority inversion** is never mentioned, although it is the classic failure mode of mutexes.
- Semaphores are said to be implemented as [[system-call|system calls]], but the notes do not say
  how the atomicity of `acquire`/`release` itself is guaranteed inside the [[kernel]].
  > [!question] Unverified — presumably by [[test-and-set]] or by disabling interrupts.

## Sources

- [[lecture-bs-3]] (p01–p12, p22–p25)
