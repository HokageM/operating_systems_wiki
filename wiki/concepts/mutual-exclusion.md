---
title: Mutual Exclusion
type: concept
tags: [concurrency, synchronisation, mutual-exclusion]
chapter: "03"
sources: [lecture-bs-3]
created: 2026-09-30
updated: 2026-09-30
---

# Mutual Exclusion (wechselseitiger Ausschluss)

## Definition

**Mutual exclusion** is a procedure that ensures that **at any point in time at most one process
is inside its [[critical-section]]**, i.e. that the concurrent accesses of several processes to a
shared [[resource]] are serialised.

## Context

Belongs to [[concurrency-and-synchronisation]]. Mutual exclusion is the answer to the
[[race-condition]]: by admitting one strand at a time it gives the [[critical-section]] the
[[atomic-operation|atomicity]] that the hardware does not provide.

It is also, uncomfortably, the **first of the four [[deadlock]] conditions** — the cure for races
is the precondition for deadlocks.

## How it works

### The four requirements

Every proposed procedure must satisfy all four. They are the grading rubric for exam answers.

| # | Requirement | German | Meaning |
|---|---|---|---|
| 1 | **Correctness** | Korrektheit | at most one process in the critical section at a time |
| 2 | **Progress** | Fortschrittsbedingung | a process *outside* the critical section must not prevent another from entering |
| 3 | **Fairness** | Fairness | every waiting process eventually gets its turn |
| 4 | **No starvation** | kein Verhungern | no process waits forever ([[starvation]]) |

Requirement 2 is the one that kills the naive solutions: strict alternation is perfectly correct
and still unusable, because a process that never wants to enter blocks the other forever.

### The implementations, in the order the chapter presents them

1. **Disabling [[interrupt|interrupts]]** — correct on a single CPU, but only available in
   [[user-mode-and-kernel-mode|kernel mode]], it disables the [[scheduler]], and it does nothing
   on a multiprocessor.
2. **Lock variables** (Sperrvariablen) — a plain `boolean locked` fails, because testing and
   setting it is itself a [[race-condition]].
3. **Strict alternation** (`turn`) — correct, violates progress.
4. **[[petersons-algorithm]]** — correct in pure software for two processes.
5. **[[test-and-set]]** — correct with one atomic hardware instruction.
6. **[[spin-lock]]** and **[[ticket-spin-lock]]** — the busy-waiting packagings.
7. **[[mutex]]**, **[[semaphore]]**, **[[monitor]]** — the blocking ones the OS offers.

All of them present the same two-call [[lock]] interface to the programmer.

## Related

- [[critical-section]] — what is being protected.
- [[atomic-operation]] — what it simulates.
- [[race-condition]] — what it prevents.
- [[lock]] — its interface.
- [[deadlock]] — what it enables, as Coffman condition 1.
- [[starvation]] — the failure of requirement 4.

## Common confusions

- **Correctness is not enough.** Three of the four requirements are about *liveness*, not safety.
  An exam answer that only checks "can two processes be inside at once?" is half an answer.
- **Mutual exclusion is a property, not a primitive.** A [[mutex]] provides it; so does a binary
  [[semaphore]], a [[monitor]] and a [[spin-lock]]. Do not equate the property with one tool.
- **Bigger critical sections are not safer, only slower.** Extending the section trivially
  preserves correctness while destroying the concurrency that motivated the design.

## Sources

- [[lecture-bs-3]] (p01, p02, p03, p22)
