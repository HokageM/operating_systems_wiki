---
title: Priority Scheduling
type: algorithm
tags: [process, scheduling]
chapter: "05"
sources: [lecture-bs-5]
created: 2026-09-30
updated: 2026-09-30
---

# Priority Scheduling

## Purpose

Assign every process a **priority** reflecting its relative importance and always run the
highest-priority Ready process. Unlike the other three algorithms of the chapter this one exists in
**both a preemptive and a non-preemptive variant**, which is exactly why it is the one most likely
to be examined.

## The three classification axes

Priorities themselves are classified along three independent axes (p03):

| Axis | Options |
|---|---|
| **origin** | **external** (set from outside, e.g. by the user or by payment) or **internal to the OS** (derived from measured behaviour) |
| **change over time** | **static** (fixed at start) or **dynamic** (adjusted at runtime) |
| **withdrawal** | **preemptive** or **non-preemptive** |

## Procedure

### Non-preemptive variant

1. Every process receives a **fixed priority**.
2. The Ready process with the **highest priority** is selected.
3. **Ties are broken by [[first-come-first-served|FCFS]]**.
4. It runs until it terminates or blocks — a higher-priority arrival must wait.

Consequence: **processes with low priority starve.** The alternative given in the notes is to
**increase the priority with increasing "age"** of the process — see [[ageing]].

### Preemptive variant

1. The process receives its priority at start and is **classified into the [[ready-queue]] by
   priority**.
2. The highest-priority Ready process is served.
3. **A process arriving with a higher priority interrupts the running process**, which is
   **re-inserted into the queue** at its priority position.

### The mixed form (Mischform)

The combination used by real systems: the CPU is withdrawn **either on expiry of a
[[time-slice]] or when a higher-priority process becomes ready**. In effect this is
[[round-robin]] within each priority level plus preemption between levels.

## Worked example

The notes give no Gantt chart for priorities; exercise S-2 covers FCFS, SPN and RR only. The
mechanism to be able to reproduce is the **re-insertion rule**: on preemption the interrupted
process does *not* go to the back of the whole queue (as in [[round-robin]]) but back into its
priority class.

## Complexity / cost

O(1) with one [[ready-queue]] per priority level (the usual implementation — pick the head of the
highest non-empty queue), O(n) with a single unsorted list. Dynamic priorities add the cost of
recomputing keys, which is why [[ageing]] is normally done periodically rather than continuously.

## Pros and cons

| | |
|---|---|
| (+) | expresses importance directly — the only algorithm here that can encode "this matters more" |
| (+) | the preemptive variant gives short response times to important interactive jobs |
| (−) | **low-priority processes starve** ([[starvation]]) unless [[ageing]] is added |
| (−) | not fair in the [[round-robin]] sense: no share of CPU time is guaranteed |
| (−) | the preemptive variant costs a [[process-switch]] on every high-priority arrival |

## Compared to

| | Selection key | Preemptive | Starvation |
|---|---|---|---|
| [[first-come-first-served]] | arrival time | no | no |
| [[shortest-process-next]] | service time | no | yes (long processes) |
| **Priority** | priority | **either** | yes (low priorities) |
| [[round-robin]] | queue position | yes | no |

[[shortest-process-next|SPN]] is the special case in which the priority is the inverse of the
service time; [[first-come-first-served|FCFS]] is the special case in which all priorities are
equal. Priority scheduling is therefore the general frame, and the other two are instances of it.

## Sources

- [[lecture-bs-5]] (p02, p03, p05, p07, p08)
