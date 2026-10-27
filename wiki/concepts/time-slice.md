---
title: Time Slice (Quantum)
type: concept
tags: [process, scheduling]
chapter: "05"
sources: [lecture-bs-5]
created: 2026-09-30
updated: 2026-09-30
---

# Time Slice (Quantum)

## Definition

The **time slice** (Zeitscheibe), also called the **quantum** and written **q**, is the fixed
duration for which a process is given the processor under a **time-slice based** [[scheduling]]
strategy. When it expires the process is preempted.

Time-slice based scheduling is **always preemptive** — a time slice *is* preemption.

## Context

Belongs to [[scheduling]], and is the one parameter of [[round-robin]]. It is realised by a
**[[clock-interrupt]]**: the timer fires, the handler decrements the remaining quantum, and at zero
it invokes the [[scheduler]].

## The trade-off

This is the "critical point" the notes name explicitly, and it is symmetrical:

| q | effect |
|---|---|
| **too small** | very frequent [[process-switch|process switches]]; a large share of CPU time goes into switching rather than application code → **CPU utilisation sinks** |
| **too large** | processes wait a long time for their turn → **bad response times**; at q → ∞ the strategy degenerates into [[first-come-first-served]] |

The quiz on p07 states the positive direction: *"The utilisation of the CPU by application code
**rises** with growing q"* — because the fixed cost of a switch is amortised over a longer useful
run.

The bound that makes the trade-off concrete: with **n** Ready processes, no process waits longer
than **(n − 1) · q**. Doubling q doubles the worst-case response time.

## Related

- [[round-robin]] — the algorithm defined by q.
- [[clock-interrupt]] — the hardware that implements it.
- [[preemptive-and-non-preemptive-scheduling]] — time slicing is one of the two ways preemption is
  triggered.
- [[process-switch]] — the cost being amortised.
- [[page-size]] — the same shape of trade-off one layer down: a parameter whose two failure modes
  point in opposite directions.

## Common confusions

- **q is an upper bound, not a fixed cost.** A process that blocks or terminates gives the CPU back
  early; it does not "use up" the rest of its slice.
- **A larger q is not simply better.** Higher application-code utilisation is bought with worse
  response times, and response time is the [[scheduling-goals|decisive requirement]] of an
  interactive system.

## Sources

- [[lecture-bs-5]] (p02, p06, p07, p09)
