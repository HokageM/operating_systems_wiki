---
title: Deadlock
type: concept
tags: [deadlock, concurrency, resources]
chapter: "03"
sources: [lecture-bs-3]
created: 2026-09-30
updated: 2026-09-30
---

# Deadlock (Verklemmung)

## Definition

A **deadlock** is a state of a **set of processes** in which

1. **every process of the set waits for an event**, and
2. **this event can only be caused by another process of the same set**.

Both clauses are required, and the exam answer needs both: it is not enough to say "the processes
wait for each other" — the point is that the awaited event can come *only* from inside the set,
so the set can never make progress.

## Context

Belongs to [[deadlocks]]. A deadlock is the price of blocking synchronisation: [[mutual-exclusion]]
is literally the first of its four necessary conditions.

## How it works

### The four necessary conditions

All four must hold **at the same time**:

| # | Condition | German |
|---|---|---|
| 1 | **Mutual exclusion** — a resource is used by one process at a time | wechselseitiger Ausschluss |
| 2 | **Hold and wait** — a process holds resources while requesting further ones | Belegungs- und Wartebedingung |
| 3 | **No preemption** — resources cannot be taken away, only given up voluntarily | Ununterbrechbarkeit / kein Entzug |
| 4 | **Circular wait** — a closed chain of processes, each waiting on the next | zirkuläres Warten |

Excluding any one of them makes deadlock impossible — this is the basis of
[[deadlock-prevention]].

### Where it comes from

The joint progress diagram (gemeinsames Fortschrittsdiagramm) for two processes that each need
two resources in **opposite order** shows a shaded region which, once entered, leads
**unavoidably** to a deadlock — no later scheduling decision can escape it. Deadlock is therefore
a property of the *request pattern*, not of bad luck in scheduling.

For single-instance [[consumable-and-reusable-resources|reusable resources]], deadlock is exactly
a **cycle in the [[resource-allocation-graph]]**. With multiple instances, a cycle is only
necessary and the [[deadlock-detection-marking-algorithm]] is required.

### Deadlock versus livelock

Deadlocked processes wait **passively** — they sit in **Blocked** in the
[[process-state-model]] and consume no CPU. A [[livelock]] waits **actively**, cycling between
**Ready** and **Running**. This one-line distinction is the standard exam answer.

## Related

- [[livelock]] — the active twin.
- [[starvation]] — permanent unfairness, without a cycle.
- [[resource-allocation-graph]] — the single-instance model.
- [[deadlock-prevention]], [[deadlock-detection]], [[deadlock-avoidance]],
  [[deadlock-recovery]] — the four responses.
- [[dining-philosophers]] — the standard illustration.
- [[mutex]], [[semaphore]] — the primitives whose misuse produces deadlocks.
- [[consumable-and-reusable-resources]] — what is being waited for.

## Common confusions

- **A deadlock needs at least two processes**, and the definition is about the *set*, not about
  any individual process. A single process blocked forever on a semaphore nobody will release is
  not a deadlock by this definition — it is [[starvation]].
- **Deadlock ≠ hang.** A process spinning forever is livelocked or starving, not deadlocked.
- **A cycle in the resource graph is only sufficient for single-instance resources.** With several
  instances per resource, a cycle can exist without a deadlock.
- **The four conditions are necessary, not sufficient.** All four can hold without any deadlock
  currently existing — that is the difference between [[deadlock-prevention]] (exclude a
  condition) and [[deadlock-avoidance]] (keep the state [[safe-state|safe]]).

## Sources

- [[lecture-bs-3]] (p13, p14, p23, p26, p27, p28)
