---
title: FCFS (First Come First Served)
type: algorithm
tags: [process, scheduling]
chapter: "05"
sources: [lecture-bs-5]
created: 2026-09-30
updated: 2026-09-30
---

# FCFS (First Come First Served)

## Purpose

The simplest [[scheduling]] policy: serve processes in the order in which they became **Ready**.
Also written **FIFO** in the notes, because the [[ready-queue]] is literally managed as a FIFO
list.

## Procedure

1. A process that becomes Ready is appended to the end of the [[ready-queue]].
2. The process at the head gets the processor and **runs to completion before a new one begins**.
3. **Exception:** if the running process blocks, a switch occurs anyway — the CPU is never left
   idle while someone is Ready.

FCFS is **non-preemptive** (see [[preemptive-and-non-preemptive-scheduling]]): nothing but
termination or blocking takes the CPU away.

## Worked example

Exercise S-2 (p30). Arrival / service times:

| Process | arrives | runs |
|---|---|---|
| B | 0 | 3 |
| D | 1 | 6 |
| A | 2 | 4 |
| C | 4 | 2 |

Served strictly in arrival order B, D, A, C:

```
t: 0    3        9      13    15
   | B  |   D    |  A   | C  |
```

Waiting times: B 0, D 2, A 7, C 9 → **mean waiting time 4.5**.

Note what happened to **C**: it only needs 2 time units but waits 9, because it arrived behind the
6-unit process D. That is the whole criticism of FCFS in one number.

## The lecture's criticism

- **Disadvantages short processes.** A short job queued behind a long one waits for the whole long
  job.
- **Favours CPU-heavy processes over I/O-heavy ones.** The example in the notes: process A is
  compute-intensive (next I/O in 30 s), B and C are I/O-heavy with compute phases of 30 ms. If A
  goes first, B and C sit idle for 30 seconds and the devices they would have driven sit idle too.
- **Not suitable on its own for a single-processor system** — in practice FCFS is used **in
  combination with priorities**, as the tie-break rule inside one priority level (see
  [[priority-scheduling]]).
- The **order of arrival strongly influences the average waiting time**, which means the
  performance of FCFS is not a property of the algorithm at all but of the workload.

## Complexity / cost

O(1) per decision — take the head of the queue. Zero bookkeeping: no priorities, no timers, no
knowledge of service times. It is the cheapest possible scheduler.

## Pros and cons

| | |
|---|---|
| (+) | trivially simple, no parameters, no knowledge of the future required |
| (+) | **no [[starvation]]** — every process eventually reaches the head |
| (−) | disadvantages short processes (convoy effect) |
| (−) | favours CPU-bound over I/O-bound processes, wasting devices |
| (−) | unusable alone for interactive systems: no bound on response time |

## Compared to

| | Selects | Mean waiting time in S-2 |
|---|---|---|
| **FCFS** | longest waiting | 4.5 |
| [[shortest-process-next]] | shortest service time | minimal, by construction |
| [[round-robin]] | next in the queue, *q* = 2 | worse than SPN, but fair |

Round Robin with an infinitely large [[time-slice]] **is** FCFS — the two are the endpoints of the
same axis.

## Sources

- [[lecture-bs-5]] (p02, p04, exercise S-2 on p30)
