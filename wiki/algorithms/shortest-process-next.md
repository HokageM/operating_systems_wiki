---
title: SPN (Shortest Process Next)
type: algorithm
tags: [process, scheduling]
chapter: "05"
sources: [lecture-bs-5]
created: 2026-09-30
updated: 2026-09-30
---

# SPN (Shortest Process Next)

## Purpose

Select the Ready process with the **smallest processing-time requirement** (kleinster
Bearbeitungszeitbedarf). This provably **guarantees the shortest mean waiting time** of any
non-preemptive policy.

## Procedure

1. Among all Ready processes, pick the one with the smallest remaining service time.
2. **Ties are broken by [[first-come-first-served|FCFS]]**.
3. Run it to completion (SPN is **non-preemptive** — see
   [[preemptive-and-non-preemptive-scheduling]]).

The decisive precondition: **the processing time must be known**. That is why SPN is listed among
the procedures for **batch jobs with known service times** and is not directly usable for
interactive work.

## Worked example

Exercise S-2 (p30). Arrival / service times: A 2/4, B 0/3, C 4/2, D 1/6.

- t = 0: only B is Ready → run **B** (0–3).
- t = 3: Ready are D (6) and A (4) → run **A** (3–7).
- t = 7: Ready are D (6) and C (2) → run **C** (7–9).
- t = 9: run **D** (9–15).

```
t: 0    3     7    9        15
   | B  |  A  | C  |   D    |
```

Waiting times: B 0, A 1, C 3, D 8 → **mean waiting time 3.0**, against 4.5 for
[[first-come-first-served|FCFS]] on the same workload.

And note the price: **D, the longest process, now finishes last and waits longest.** With a steady
supply of short jobs it would never run at all.

## Why the mean waiting time is minimal

Informally: the mean waiting time is the sum over all processes of "how many processes are still
waiting while I run", weighted by my own length. Putting a long job first makes *everyone* behind
it wait for its whole length; putting short jobs first makes only a few wait, and only briefly.
Sorting by length is therefore the optimum — the same exchange argument as in the scheduling of
tasks on one machine.

## Complexity / cost

Selecting the minimum costs O(n) per decision over the [[ready-queue]], or O(log n) with a heap.
The real cost is not computational — it is **knowing the service times in advance**.

## Pros and cons

| | |
|---|---|
| (+) | **guarantees the shortest mean waiting time** |
| (+) | avoids the preference of long processes that [[first-come-first-served|FCFS]] shows |
| (−) | **requires the processing time to be known** — rarely true outside batch systems |
| (−) | **long processes can [[starvation\|starve]]** |
| (−) | non-preemptive, so a newly arrived short job still waits for the running one |

## Compared to

| | Knowledge required | Fair | Starvation |
|---|---|---|---|
| [[first-come-first-served]] | none | — | no |
| **SPN** | service times | no | **yes** (long processes) |
| [[priority-scheduling]] | priorities | no | **yes** (low priorities) |
| [[round-robin]] | none | **yes** | no |

SPN and [[priority-scheduling]] are structurally the same algorithm with a different key: SPN uses
"1 / service time" as the priority. Accordingly they share the same failure mode and, in principle,
the same cure — [[ageing]].

## Sources

- [[lecture-bs-5]] (p02, p05, exercise S-2 on p30)
