---
title: Ready Queue
type: concept
tags: [process, scheduling]
chapter: "05"
sources: [lecture-bs-5]
created: 2026-09-30
updated: 2026-09-30
---

# Ready Queue

## Definition

The data structure holding all processes in state **Ready** (see [[process-state-model]]) — the
set from which the [[scheduler]] makes its choice. Every [[scheduling]] algorithm of chapter 05 is
in essence a rule for inserting into and removing from this queue.

## Context

Belongs to [[scheduling]] and to [[processes]]. Its entries are (pointers to)
[[process-control-block|process control blocks]]; the queue itself lives in the kernel.

## Implementations per algorithm

| Algorithm | Structure | Insert | Remove |
|---|---|---|---|
| [[first-come-first-served]] | **FIFO list** | at the end | from the head |
| [[shortest-process-next]] | any | anywhere | minimum service time |
| [[priority-scheduling]] | sorted by priority (or one list per level) | at the process's priority position | highest priority |
| [[round-robin]] | **linear list**, used cyclically | at the end — both for new arrivals *and* for preempted processes | from the head |

The Round-Robin insertion rule is the one exercise S-2 c) tests: **a process that has just been
preempted goes behind anything that arrived while it was running.**

## Related

- [[process-state-model]] — only Ready processes are in the queue; Blocked processes wait
  elsewhere, typically in a queue belonging to the resource they wait for.
- [[scheduler]] — the queue's only reader.
- [[semaphore]] — carries its own waiting queue, and moving a process from there to the ready
  queue is what `release()` does.
- [[starvation]] — what happens when a process is in the queue but never selected.

## Common confusions

- **Ready queue ≠ the set of all processes.** Running, Blocked and suspended processes are not in
  it. In the 7-state model there is also a *ready-suspended* set, which lives on persistent storage
  (see [[swapping]]).
- "Queue" is loose terminology: only under FCFS and Round Robin is it actually FIFO.

## Sources

- [[lecture-bs-5]] (p04, p06, p07, p08, exercise S-2 on p30)
