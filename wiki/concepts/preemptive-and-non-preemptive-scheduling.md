---
title: Preemptive and Non-Preemptive Scheduling
type: concept
tags: [process, scheduling]
chapter: "05"
sources: [lecture-bs-5]
created: 2026-09-30
updated: 2026-09-30
---

# Preemptive and Non-Preemptive Scheduling

## Definition

- **Preemptive** (verdrängend / präemptiv) — **the CPU can be taken away from the process at any
  time.**
- **Non-preemptive** (nicht-verdrängend) — **the process holds the CPU until it ends or yields it
  voluntarily.**

This is the central distinction of the [[scheduling]] chapter and the one asked verbatim in the
exercise recap.

## Context

Belongs to [[scheduling]]. Every strategy sits on one side of this line:

| non-preemptive | preemptive |
|---|---|
| [[first-come-first-served]] | [[round-robin]] |
| [[shortest-process-next]] | [[priority-scheduling]] (preemptive variant) |
| [[priority-scheduling]] (non-preemptive variant) | the **mixed form** |
| [[cooperative-scheduling]] | |

## The trade-off, in the notes' own words

> **Preemptive = greater overhead but a higher number of processes.**

- Every preemption is a full [[process-switch]], with all eight of its steps — pure overhead that
  produces no application work.
- In exchange, no single process can monopolise the processor, so many more processes can make
  progress and the system stays responsive.

**Non-preemptive scheduling is not suitable for systems with interactive users**: one runaway
process freezes the whole machine, because nothing can take the CPU back. Non-preemptive
procedures are therefore listed as applying to **batch jobs with known service times**.

## How preemption is realised

Only two things can take the CPU from a running process:

1. A **[[clock-interrupt]]** — the basis of every [[time-slice]] based strategy.
2. An **[[interrupt]] that makes a higher-priority process Ready** — the basis of preemptive
   [[priority-scheduling]].

Both are hardware events. Without interrupt hardware, preemption is impossible, which is why
[[cooperative-scheduling]] was the norm on early systems.

## Related

- [[cooperative-scheduling]] — voluntary yielding, the non-preemptive way to share a CPU.
- [[time-slice]] — the quantum that defines *how often* preemption happens.
- [[process-switch]] — what a preemption costs.
- [[process-state-model]] — preemption is the **Running → Ready** transition ("Timeout"), as
  opposed to blocking, which is **Running → Blocked**.
- [[race-condition]] — preemption at an arbitrary point is precisely what creates races; see also
  [[critical-section]].

## Common confusions

- **Preemption is not blocking.** A preempted process is **Ready** and could run immediately; a
  blocked process is **Blocked** and cannot. Both leave the Running state, via different arrows.
- **Non-preemptive does not mean "no switching".** Even under
  [[first-come-first-served|FCFS]] a switch occurs when the running process **blocks** — the CPU is
  never left idle. It just cannot be *taken*.

## Exam relevance

Exercise S-1 b) asks for the difference and adds two riders that must be stated: non-preemptive is
**not suitable for interactive users**, and preemptive causes **greater overhead but allows a
higher number of processes**.

## Sources

- [[lecture-bs-5]] (p01, exercise S-1 b on p28)
