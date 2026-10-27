---
title: Round Robin
type: algorithm
tags: [process, scheduling]
chapter: "05"
sources: [lecture-bs-5]
created: 2026-09-30
updated: 2026-09-30
---

# Round Robin

## Purpose

The standard **time-slice based**, always **preemptive** [[scheduling]] policy: every Ready process
gets the processor for a fixed duration **q** in a fixed cyclic sequence. It is the algorithm that
makes an interactive system feel responsive, and it is **fair** in a precise, provable sense.

## Procedure

The notes give the *Vorgehen* as three numbered steps (p08):

1. The process at the head is **removed from the [[ready-queue]]** and the processor is assigned
   to it.
2. **At the end of the time slice, or on blocking, a switch occurs** and the process is
   **inserted at the end of the queue**.
3. Jobs arriving during or at the end of the time slice are likewise **inserted at the end of the
   queue**.

### Implementation

- The time slice is realised by a **[[clock-interrupt]]** (Zeitgeber-Interrupt). The handler
  decrements the remaining quantum and, on zero, invokes the [[scheduler]].
- The [[ready-queue]] is a **linear list**, used cyclically.
- An earlier switch happens if the process blocks or terminates — the quantum is an upper bound,
  not a fixed cost.

## The fairness guarantee

With **n** processes:

- each gets **1/n of the CPU time, in units of size q**;
- **no process waits longer than (n − 1) · q** for its next turn.

These two statements are the reason Round Robin is called fair, and they are the sentences to
reproduce in the exam. Note that fairness here means *equal share of time*, not *equal completion
time* — a long process still finishes last.

## Worked example

Exercise S-2 c) (p30), **q = 2**. Arrival / service: A 2/4, B 0/3, C 4/2, D 1/6.

The queue evolves by appending both newly arrived and just-preempted processes at the end:

| t | running | ready queue after the step |
|---|---|---|
| 0–2 | B | B (D arrives at 1) → D, B |
| 2–4 | D | A arrives at 2 → B, A; D appended → B, A, D |
| 4–6 | B (last unit at 5) | C arrives at 4 → A, D, C |
| 6–8 | A | D, C, A |
| 8–10 | D | C, A, D |
| 10–12 | C | A, D |
| 12–14 | A | D |
| 14–16 | D | D |
| 16–17 | D | — |

The point the exercise tests is the **ordering rule**, not the arithmetic: at each preemption the
running process goes to the *back*, behind anything that arrived in the meantime.

Verdict recorded in the notes: **fair, no [[starvation]] possible**.

## The choice of q

The critical point of the whole algorithm:

| q | consequence |
|---|---|
| **too small** | frequent [[process-switch|process switches]]; the share of CPU time spent on switching rises and **CPU utilisation sinks** |
| **too large** | **bad response times**; in the limit q → ∞ Round Robin *is* [[first-come-first-served]] |

The quiz on p07 makes the first row explicit: with growing q, **the utilisation of the CPU by
application code rises**, because proportionally less time goes into switching. See
[[time-slice]].

## Complexity / cost

O(1) per decision. The cost is the switching itself: one [[process-switch]] per expired quantum,
which is why q is chosen orders of magnitude larger than the switch time.

## Pros and cons

| | |
|---|---|
| (+) | **simple and widely used** |
| (+) | **fair** — 1/n of the CPU, bounded waiting time |
| (+) | **no starvation**, no knowledge of service times required |
| (−) | worse mean waiting time than [[shortest-process-next]] |
| (−) | the whole behaviour depends on one badly-conditioned parameter, q |
| (−) | treats an I/O-bound process the same as a compute-bound one, although it uses only a fraction of its quantum |

## Compared to

Round Robin is the preemptive counterpart of [[first-come-first-served]] — same queue, same
ordering, plus a timer. Combined with priorities (one Round-Robin queue per priority level) it
becomes the **mixed form** described in [[priority-scheduling]], which is what real operating
systems run.

## Sources

- [[lecture-bs-5]] (p02, p06–p09, exercise S-2 on p30)
