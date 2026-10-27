---
title: Scheduler
type: concept
tags: [process, scheduling]
chapter: "01"
sources: [lecture-bs-1, lecture-bs-5]
created: 2026-09-30
updated: 2026-09-30
---

# Scheduler

## Definition

The **scheduler** is the program which **determines when processes are interrupted and to which
process is switched** (bestimmt, wann Prozesse unterbrochen werden und zu welchem Prozess
gewechselt wird).

## Context

Belongs to [[processes]]. It is the *decider*; the [[dispatcher]] is the *executor*.

## How it works

The scheduler chooses among the processes in state **Ready** (see [[process-state-model]]) — never
among Blocked or suspended ones. Its decision is step 4 of the [[process-switch]]. Inputs are the
scheduling and state information held in each [[process-control-block]], including the process
priority.

Its effectiveness is measured by metrics such as [[cpu-utilisation]], and more generally against
the [[scheduling-goals]].

## Which strategy?

Chapter 05 supplies the policies this page could not name. The scheduler reads the
[[ready-queue]] and applies one of:

| Non-preemptive | Preemptive |
|---|---|
| [[first-come-first-served]] | [[round-robin]] |
| [[shortest-process-next]] | [[priority-scheduling]] (preemptive variant) |
| [[priority-scheduling]] (non-preemptive variant) | the mixed form |

See [[scheduling]] for the full treatment and
[[preemptive-and-non-preemptive-scheduling]] for the distinction that organises them.

## Related

- [[scheduling]] — the topic; the strategies the scheduler may implement.
- [[ready-queue]] — the set it chooses from.
- [[clock-interrupt]] — what invokes it in a time-slice based system.
- [[dispatcher]] — carries out what the scheduler decides.
- [[process-switch]] — step 4 is the scheduler's.
- [[cpu-utilisation]] — the metric it optimises.
- [[interrupt-prioritisation]] — the same decision problem one level down, for interrupts.
- [[starvation]] — the failure mode an unfair scheduler produces.
- [[race-condition]] — the scheduler's freedom to preempt anywhere is what makes races possible.

## Common confusions

- **Scheduler vs. dispatcher** — see [[dispatcher]].
- **The scheduler is not a scheduling strategy.** Round Robin, FCFS etc. are the *policies* it may
  implement. Chapter 01 named none of them; [[lecture-bs-5]] supplies them all.

## Sources

- [[lecture-bs-1]] (p09, p19)
- [[lecture-bs-5]] (p01) — resolves the chapter-01 open question: the concrete strategies.
