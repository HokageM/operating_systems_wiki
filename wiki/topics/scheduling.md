---
title: Scheduling
type: topic
tags: [process, scheduling]
chapter: "05"
sources: [lecture-bs-5]
created: 2026-09-30
updated: 2026-09-30
---

# Scheduling

## Summary

**Process scheduling** (Prozess-Scheduling) is the **planning of the temporal assignment of
processes** to the processor. Chapter 01 introduced the two programs that do it — the
[[scheduler]] decides *which* process runs next, the [[dispatcher]] performs the switch — but left
the policies open. This chapter fills that gap: it names the goals, the two axes along which every
strategy is classified, and four concrete algorithms.

The whole subject can be compressed into one table:

|  | non-preemptive | preemptive |
|---|---|---|
| **priority-based** | [[priority-scheduling]] (non-preemptive variant) | [[priority-scheduling]] (preemptive variant) |
| **time-slice-based** | — (impossible: a time slice *is* preemption) | [[round-robin]] |
| **arrival/length-based** | [[first-come-first-served]], [[shortest-process-next]] | — |

## Why it exists

The processor is the one [[resource]] that cannot be shared in space, only in time. Every
multiprogramming system therefore needs a rule for dividing it, and the rule cannot satisfy
everyone — see [[scheduling-goals]], where the user's wishes and the system's wishes are listed
separately precisely because they conflict.

## Key concepts

- [[scheduling-goals]] — the user's goals vs. the operating system's goals.
- [[preemptive-and-non-preemptive-scheduling]] — the central distinction of the chapter.
- [[cooperative-scheduling]] — preemption by voluntary yielding.
- [[time-slice]] — the quantum *q*, and what happens when it is chosen badly.
- [[clock-interrupt]] — the hardware that makes time slicing possible.
- [[ready-queue]] — the data structure every algorithm manipulates.
- [[ageing]] — the standard cure for [[starvation]] under priorities.
- [[starvation]] — the failure mode of SPN and of static priorities.

## Mechanisms / algorithms

The four algorithms, in the order the notes present them:

| Algorithm | Selects | Preemptive? | Fair? | Starvation |
|---|---|---|---|---|
| [[first-come-first-served]] | the longest-waiting process | no | — | no |
| [[shortest-process-next]] | the shortest service time | no | — | **yes**, long processes |
| [[priority-scheduling]] | the highest priority | either | no | **yes**, low priorities (cure: [[ageing]]) |
| [[round-robin]] | the next in the queue | yes | **yes** | no |

Non-preemptive procedures are presented as the ones that apply to **batch jobs with known service
times** — FCFS, SPN and non-preemptive priority. Interactive systems need preemption.

There is also a **mixed form** (Mischform): the CPU is withdrawn *either* when the time slice
expires *or* when a higher-priority process becomes ready. This is what real systems use, and it
is how [[priority-scheduling]] and [[round-robin]] are combined.

## Trade-offs

- **Overhead vs. process count.** Preemption costs a [[process-switch]] every time, but allows a
  higher number of processes to make progress. This exact sentence is an exam answer — see
  [[preemptive-and-non-preemptive-scheduling]].
- **Fairness vs. throughput.** SPN provably minimises the mean waiting time, and provably starves
  long processes. Round Robin is fair and provably worse on mean waiting time.
- **Response time vs. utilisation.** The [[time-slice]] trade-off in one line: *q* too small →
  constant switching, CPU utilisation sinks; *q* too large → bad response times, and in the limit
  Round Robin degenerates into FCFS.
- **Policy vs. mechanism.** The algorithms here are policies; the [[dispatcher]] and the
  [[clock-interrupt]] are mechanisms. The exam asks for both, separately.

## Exam relevance

Very high — exercise S-2 is a pure drawing exercise:

- **Draw the Gantt chart** for FCFS, SPN and RR with *q* = 2 over a given arrival/service table,
  then state the disadvantage and whether starvation is possible. The worked example (A: arrives 2,
  runs 4; B: 0/3; C: 4/2; D: 1/6) is reproduced in all three algorithm pages.
- **Definitions**: dispatcher vs. scheduler (ch.01), preemptive vs. non-preemptive, the decisive
  performance requirement of an interactive system.
- **The 8 steps of a [[process-switch]]** and the [[thread-switch]] delta — asked in S-1 c) and d).

## Open questions

> [!question] Unverified — not in the notes
> Multi-level feedback queues, lottery scheduling, real-time scheduling (EDF, rate-monotonic) and
> multiprocessor scheduling are never mentioned. Neither is the **shortest remaining time**
> (preemptive SPN) variant, although it is the obvious counterpart to SPN.

## Sources

- [[lecture-bs-5]] — lecture part p01–p09, exercise recap S-1 and S-2.
