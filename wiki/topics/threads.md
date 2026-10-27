---
title: Threads
type: topic
tags: [thread, concurrency, multithreading, smt]
chapter: "02"
sources: [lecture-bs-2, lecture-bs-5]
created: 2026-09-30
updated: 2026-09-30
---

# Threads

## Summary

A [[thread]] is a sequential strand of execution inside a [[process]] that shares the code and
data segment with the other threads of that process. Threads exist because
**process creation and process switching are expensive** — a [[thread-switch]] only exchanges the
[[thread-control-block]], whereas a [[process-switch]] also swaps memory tables, memory areas and
the cache. Each thread owns a program counter, registers, a thread-local stack and a pointer to a
thread-local memory area; everything else belongs to the process. Threads run through four
transitions — creation, blocking, unblocking, termination ([[thread-state-model]]) — and are
programmed with the [[pthread-api]]. On the hardware side,
[[simultaneous-multithreading|SMT]] splits one physical processor into two logical ones so that
two instruction streams run in parallel without any switching at all.

## Why it exists

Processes give isolation, and isolation is exactly what makes them expensive: a private address
space must be created on [[fork]] and swapped on every [[process-switch]], taking the cache with
it. But many programs need concurrency *within* one application, where isolation is not wanted —
the parts want to share the same data. Threads drop the isolation and keep the concurrency: cheap
to create, cheap to switch, and able to communicate through plain variables instead of
[[pipe|pipes]] or shared memory.

## Key concepts

- [[thread]] — the definition and its advantages
- [[thread-control-block]] — what is private to a thread
- [[thread-state-model]] — creation, blocking, unblocking, termination
- [[thread-switch]] — why it beats the [[process-switch]]
- [[pthread-api]] — `pthread_create`, `pthread_exit`, `pthread_join`
- [[simultaneous-multithreading]] — the hardware form
- [[signal]] and [[signal-handler]] — revisited in depth by this chapter's exercise sheet

The sequel is [[concurrency-and-synchronisation]]: because threads share the data segment, every
shared variable is a potential [[race-condition]], and chapter 3 supplies the
[[mutual-exclusion]] machinery to deal with it.

## Mechanisms / algorithms

### Threads vs. processes

| | Process | Thread |
|---|---|---|
| **Consists of** | code, data, stack (Stapelspeicher), file table (Dateitabelle), interrupt-vector table, HW state | program counter, registers, thread-local stack, pointer to thread-local memory area |
| **Shares** | only the **code** can be shared | **data, file table, interrupt-vector table** (plus code) |
| **Communication** | via [[pipe\|pipes]], shared memory or similarly **expensive mechanisms** | **simply, via shared variables — without the OS** |
| **Switch / creation cost** | expensive | **much more efficient** |
| **Kinship (Verwandtschaft)** | parent/child tree ([[fork]], [[orphan-process]]) | **none** |

### Thread state, split by owner

- **Shared by all threads of a process:** memory state (Speicherzustand), file-system state
  (Dateisystemzustand).
- **Private to a single thread:** program counter, registers, thread-local stack, pointer to the
  thread-local memory area.

### Termination

| Process end | Thread end |
|---|---|
| `main` function is left | thread function is left |
| `exit()` is called | `pthread_exit()` |
| the last thread terminates itself | the process is terminated |

## Trade-offs

- **Sharing buys speed and costs safety.** Communication through variables needs no OS call, but
  nothing protects one thread's data from another's — where processes get isolation for free,
  threads must synchronise explicitly.
- **Cheap switching depends on staying inside one process.** The efficiency argument only holds
  between threads of the *same* process; across processes the memory tables and cache must change
  anyway.
- **No kinship means no supervision.** There is no `waitpid`-style hierarchy; forgetting
  `pthread_join()` simply kills the created thread when the process ends.
- **SMT adds logical parallelism, not physical throughput.** Two register sets share one set of
  execution units.
- **Concurrency without determinism.** As with processes, no assumption may be made about the
  temporal course of execution.

## Exam relevance

- *"Was ist ein Thread?"* — reproduce the [[thread|one-sentence definition]] verbatim; it is asked
  twice in this source alone.
- *"Wodurch wird der Zustand eines Threads beschrieben?"* — the four private items
  ([[thread-control-block]]).
- *"Was passiert bei einer Umschaltung zwischen Threads im Gegensatz zu Prozessen?"* — the
  four-row vs. one-row table in [[thread-switch]]. Very likely exam question.
- *"Threads vs. Prozesse"* — what each consists of, what each can share, how each communicates.
- *"Vorteile von Threads?"* — concurrency, simple sequential programming model, performance, cheap
  creation and switching, simple communication via shared variables.
- *"Was ist Simultaneous Multithreading?"* — [[simultaneous-multithreading|definition]] with the
  two register sets and the complex control unit.
- *"Gibt es zwischen Threads eine Verwandtschaftsbeziehung wie bei Prozessen?"* — **No.**
- *"Was passiert, wenn `pthread_join()` entfernt wird?"* — the other thread dies with the process.
- *"Wann wird ein Prozess bzw. Thread beendet?"* — the two-column table above.
- Signals: *"Was ist ein Signal?"*, *"Interrupt, Trap & Signal?"*, `sigemptyset` vs. `sigfillset`,
  and the `sleep()` restart loop ([[signal-handler]]).

## Open questions

- The notes describe thread states as four transitions rather than as a labelled state diagram. If
  the exam asks for a *Thread-Zustandsmodell* drawing, the [[process-state-model]] diagram is the
  closest available template — worth confirming against the lecture slides.
- Nothing in this source says whether the threads are **kernel-level or user-level** threads
  (1:1, N:1, M:N models). Given `pthread_create` and the ready queue, kernel-level is implied.
  > [!question] Unverified — not in the notes.
- Synchronisation primitives (mutex, semaphore, condition variables) are the obvious sequel to
  shared-address-space communication but are not in this source — **resolved by chapter 3**:
  see [[concurrency-and-synchronisation]], [[mutex]], [[semaphore]] and [[condition-variable]].
  The [[pthread-api]] page now carries the `pthread_mutex_*` and `pthread_cond_*` calls.

## Sources

- [[lecture-bs-2]] (p01–p08)
- [[lecture-bs-5]] (exercise S-1 d) — a [[thread-switch]] is the eight-step [[process-switch]] **without step 5**: no MMU preparation, because the address space is shared. This is the cleanest confirmation of why a thread switch is cheap, and it is asked directly.
