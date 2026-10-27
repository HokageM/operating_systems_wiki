---
title: Thread
type: concept
tags: [thread, concurrency, process]
chapter: "02"
sources: [lecture-bs-2]
created: 2026-09-30
updated: 2026-09-30
---

# Thread

## Definition

A **thread** is a **sequential strand of execution** (sequentieller Ausführungsstrang) **of a
[[process]] that shares the code segment and the data segment with the other threads of the same
process**.

This is the verbatim definition from the notes and it is asked in exactly this form — it appears
twice in the source (lecture part and exercise recap).

## Context

Belongs to [[threads]]. A thread is the unit of execution *inside* a [[process]]; the process
remains the unit of resource ownership.

## How it works

Every thread has its **own program counter**, so:

> thread ⟺ independent program counter (unabhängiger Programmzähler)

Private to each thread: [[processor-registers|program counter and registers]], a **thread-local
stack**, and a **pointer to a thread-local memory area**. Everything else — the address space,
the data, the open files — belongs to the [[process]] and is shared.

### Advantages (Vorteile)

- **Concurrency** (Nebenläufigkeit).
- **Simple programming**: each thread keeps the familiar sequential model.
- **Good performance.**
- **Cheap creation and cheap context switch** — see [[thread-switch]].
- **Simple communication via shared variables**, because all threads live in one address space —
  no [[pipe]] or shared-memory setup needed.

The motivation is stated directly in the notes: *process creation and process switching are
expensive* (Prozesserzeugung, Prozessumschaltung sind teuer). Threads are the cheap answer.

## Related

- [[process]] — the owner of the resources a thread runs in.
- [[thread-control-block]] — the per-thread bookkeeping record, the [[process-control-block]]'s
  little brother.
- [[thread-state-model]] — creation, blocking, unblocking, termination.
- [[thread-switch]] — why switching threads beats switching processes.
- [[pthread-api]] — the concrete Linux/POSIX interface.
- [[simultaneous-multithreading]] — the *hardware* meaning of the word, not this one.
- [[mutex]], [[semaphore]], [[condition-variable]] — chapter 3's answer to the danger of sharing
  the data segment: the shared state must be synchronised.
- [[race-condition]] — what unsynchronised sharing produces.
- [[deadlock]] — what over-synchronised sharing produces.

## Common confusions

- **Threads have no kinship relation.** Processes form a parent/child tree (see
  [[fork]], [[orphan-process]]); threads do **not** — this is asked explicitly in the exercise
  recap, and the answer is a flat "No".
- **`main()`'s thread is not special in structure.** The difference between the thread running
  `main()` and a newly created thread is only its **stack and PC** — not a different kind of
  object. It is special only in role: it is the *main thread* (Haupt-Thread) created with the
  process.
- **Sharing the data segment is the point and the danger.** It is what makes communication free,
  and also what makes synchronisation necessary.
- A thread is not "a lightweight process" in the sense of having its own address space — it has
  none of its own.

## Sources

- [[lecture-bs-2]] (p01, p02, p04, p05, p08)
