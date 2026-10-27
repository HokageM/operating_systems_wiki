---
title: Ageing
type: concept
tags: [process, scheduling]
chapter: "05"
sources: [lecture-bs-5]
created: 2026-09-30
updated: 2026-09-30
---

# Ageing

## Definition

The technique of **increasing a process's priority with its increasing "age"** — that is, with the
time it has already spent waiting. It is the standard cure for the [[starvation]] that static
[[priority-scheduling]] produces.

## Context

Belongs to [[scheduling]]. It turns a **static** priority into a **dynamic** one — the second of
the three classification axes in [[priority-scheduling]].

## How it works

A waiting process's effective priority is raised periodically (or computed as *base priority +
f(waiting time)*). Sooner or later even the lowest-priority process outranks everything else and is
scheduled. When it eventually runs, its priority is reset to its base value.

The essential property: **the effective priority of a waiting process grows without bound**, so
there is a finite time after which it must be selected. That converts "may wait forever" into "will
wait at most T" — exactly the guarantee [[starvation]] lacks.

## Related

- [[starvation]] — the problem it solves.
- [[priority-scheduling]] — where it is applied; the notes introduce it as the *alternative* to a
  purely fixed priority.
- [[shortest-process-next]] — has the same failure mode (long processes starve) and could be
  repaired the same way.
- [[ticket-spin-lock]] — the same idea at the lock level: an explicit ordering guarantee is added
  to an otherwise unfair mechanism.
- [[round-robin]] — the alternative approach: instead of repairing unfairness, avoid priorities
  altogether.

## Common confusions

- **Ageing is not fairness.** It bounds the waiting time; it does not give every process an equal
  share of the CPU the way [[round-robin]] does.
- Ageing is about the *waiting* time, not the *service* time. A process that has run a lot does not
  get a lower priority by ageing — that would be a separate, opposite mechanism.

## Sources

- [[lecture-bs-5]] (p05)
