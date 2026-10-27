---
title: "Lecture BS 2 — Threads, SMT and Signals"
type: source
tags: [lecture, thread, concurrency, smt, signals]
chapter: "02"
sources: [lecture-bs-2]
created: 2026-09-30
updated: 2026-09-30
---

# Lecture BS 2 — Threads (Multithreading von Prozessen)

- **Raw file:** `raw/lecture/bs_2.pdf`
- **Type:** lecture notes (handwritten summary, German) + exercise recap "BS-KVB-Ex2"
- **Ingested:** 2026-09-30
- **Pages:** 8 (scanned handwriting, no text layer — read via page rasterisation)

## Key takeaways

- Motivation, stated up front: **process creation and process switching are expensive**
  (Prozesserzeugung, Prozessumschaltung sind teuer). Threads are the cheap alternative.
- A [[thread]] is a **sequential strand of execution of a [[process]] that shares the code and
  data segment with the other threads of the same process**. Thread ⟺ independent program counter.
- Advantages: concurrency, a simple sequential programming model, good performance, cheap creation
  and switching, and simple communication via shared variables.
- **Threads vs. processes:** processes consist of code, data, stack, file table, interrupt-vector
  table and HW state, and can share only the **code** — they must communicate through [[pipe|
  pipes]] or shared memory. Threads share **data, file table and interrupt-vector table** and can
  exchange information **without the OS**.
- Private to a thread ([[thread-control-block]]): program counter, registers, thread-local stack,
  pointer to the thread-local memory area. Shared: memory state and file-system state.
- Four transitions in the [[thread-state-model]]: Erzeugung (the main thread is created with the
  process and put into the ready queue), Blockierung, Aufhebung der Blockierung, Beendigung.
- A [[thread-switch]] only exchanges the TCB, while a [[process-switch]] also exchanges the
  memory-management table, replaces the memory areas and flushes and reloads the cache — hence
  *viel effizienter*.
- [[simultaneous-multithreading|SMT]] splits one physical processor into **two logical
  processors** using a complex control unit and **two complete register sets**, executing two
  parallel instruction streams.
- Exercise recap Ex2 re-asks [[signal]], [[interrupt]]/[[trap]], [[system-call]] and [[thread]]
  definitions, then goes deep on [[signal-handler|signal handling]]: `sigaction`, `SIGUSR1`,
  `kill -USR1 pid`, `sigemptyset` vs. `sigfillset`, and the `sleep()` restart loop.
- Threads have **no kinship relation** (Verwandtschaftsbeziehung) like processes do; the thread
  running `main()` differs from a created thread only in **stack and PC**.

## Detailed summary

### Part 1 — Threads (p01–p03)

Page 1 opens with the cost argument against processes, gives the boxed [[thread]] definition, the
advantage list, the "thread ⟺ independent program counter" equivalence, and the
**threads vs. processes** comparison: what each consists of, that only code can be shared between
processes, that processes need expensive mechanisms ([[pipe|pipes]], shared memory) to
communicate while threads exchange information without the OS, and that thread switching and
creation are much more efficient.

Page 2 introduces [[pthread-api|`pthread_create()`]] and splits thread state into shared
(memory state, file-system state) and private ([[thread-control-block]]: PC, registers,
thread-local stack, pointer to the thread-local memory area), then lists the four
[[thread-state-model|state transitions]], including that the main thread is created together with
the process and inserted into the ready queue (Bereit-Queue).

Page 3 gives the boxed [[simultaneous-multithreading]] definition and the remark that each thread
has its own stack.

### Part 2 — Exercise recap "BS-KVB-Ex2" (p04–p08)

**2-1, questions 1–4 (p04)** restate definitions already in the wiki from chapter 1: a [[signal]]
is a system message to a running process; an [[interrupt]] is a signal that interrupts normal
program execution to carry out other processing, a [[trap]] is an interrupt triggered by an
instruction; a [[system-call]] is a method to execute OS-provided functionality, handing control
from the program to the [[kernel]]; and the [[thread]] definition again, word for word.

**Questions 5–6 (p05)** ask what describes a thread's state (the four private items) and what
happens on a thread switch as opposed to a process switch — the four-row vs. one-row comparison
now on [[thread-switch]].

**2-2 Signals (p06–p07)** notes that every process owns an interrupt-vector table specifying how
an occurring signal is to be handled, changeable via `sigaction`; then walks through raising
`SIGUSR1` (`kill -USR1 pid` in the shell, `kill(pid, SIGUSR1)` in code), the effect of a signal on
`sleep()` (`SIGALARM`), the `sigemptyset` / `sigfillset` distinction for signals arriving during
the handler, and the `while ((seconds = sleep(seconds)) != 0);` loop that guarantees a 5-second
period. All of this is on [[signal-handler]].

**2-3 Threads (p08)** answers four short questions: the `main()` thread differs from a created
thread only in stack and PC; there is **no** kinship relation between threads; removing
[[pthread-api|`pthread_join()`]] means the other thread is terminated immediately when the process
ends; and the process-end / thread-end conditions.

## New pages created

Topics: [[threads]]

Concepts: [[thread]], [[thread-control-block]], [[thread-state-model]], [[thread-switch]],
[[simultaneous-multithreading]], [[pthread-api]], [[signal-handler]]

## Pages updated

[[overview]] (ch.02 row and big picture), [[index]], [[processes]],
[[interprocess-communication]], [[process]], [[process-switch]], [[process-control-block]],
[[context-switch]], [[signal]], [[process-state-model]], [[fork]], [[pipe]],
[[processor-registers]]

## Contradictions / conflicts with existing wiki

None. Chapter 2 is consistent with chapter 1 and extends it: the process cost model from
[[process-switch]] is reused unchanged as the baseline that threads improve on, and the
[[signal]] definition is restated identically.

One terminology overlap worth noting rather than a conflict: the **interrupt-vector table** is
used in two senses in the notes — the CPU-level [[interrupt-vector-table]] from chapter 1, and a
**per-process** table describing signal reactions (p06). Recorded on [[signal-handler]].

## Open questions

- Exercise sheet 2 itself ("Ex2") is not in `raw/exercise/` — only this recap. The referenced C
  programs (the `SIGUSR1` program, the `pthread_join` program) are therefore not traceable.
- The notes do not distinguish **kernel-level vs. user-level threads**; `pthread_create` plus the
  ready queue implies kernel-level, but this is not stated.
- No **synchronisation** primitives (mutex, semaphore) appear despite the shared address space
  making them necessary — presumably chapter 3.
- No labelled **thread state diagram**, only the four transitions.
