---
title: Deadlock Prevention
type: concept
tags: [deadlock, prevention, strategy]
chapter: "03"
sources: [lecture-bs-3]
created: 2026-09-30
updated: 2026-09-30
---

# Deadlock Prevention (Deadlock-Verhinderung)

## Definition

**Deadlock prevention** makes deadlocks **structurally impossible** by designing the system so
that **at least one of the four necessary [[deadlock]] conditions can never hold**.

## Context

Belongs to [[deadlocks]]. It is the second of the four strategies (ignore, prevent, detect and
recover, avoid) and the only one that gives an absolute guarantee without runtime analysis —
which is why it is also the most restrictive.

## How it works

One condition is excluded; each choice has a price.

| Condition excluded | Technique | Drawback |
|---|---|---|
| **1. [[mutual-exclusion]]** | make resources shareable (e.g. spooling a printer) | impossible for most resources — mutual exclusion is usually required for correctness |
| **2. Hold and wait** | **preclaiming**: a process must request **all** resources at once, up front, and gets none until all are free | very poor utilisation — resources sit idle for the whole runtime; the maximum need must be known in advance |
| **3. No preemption** | **forced resource release**: a process that is refused a resource must give up everything it holds and start over | work is lost; not possible for resources with state (a half-written file, a held [[mutex]]) |
| **4. Circular wait** | **linear resource ordering**: number all resources and require that each process requests them in strictly increasing order | constrains the programmer rather than the system; can only be enforced by convention; forces unnatural request orders |

**Linear resource ordering** is the practically useful one: it costs nothing at runtime and is the
standard fix for the "two mutexes in opposite order" bug. It is also the clean solution to
[[dining-philosophers]] — if one philosopher picks up the right fork first, the cycle is broken.

## Related

- [[deadlock]] — the four conditions being attacked.
- [[deadlock-avoidance]] — the softer alternative: allow the conditions, refuse dangerous states.
- [[deadlock-detection]] — the opposite philosophy: allow it, then fix it.
- [[mutual-exclusion]] — condition 1.
- [[dining-philosophers]] — the standard application of linear ordering.

## Common confusions

- **Prevention ≠ avoidance.** Prevention removes a *condition* once and for all, statically.
  [[deadlock-avoidance]] leaves all four conditions in place and refuses individual allocations at
  runtime based on the [[safe-state]] test. This distinction is a frequent exam question.
- **Excluding one condition is enough.** All four are necessary, so breaking any single one
  suffices — there is no need to break several.
- **Preclaiming is not the banker's algorithm.** Both need maximum claims, but preclaiming grants
  everything at once at the start; the [[bankers-algorithm]] grants incrementally.

## Sources

- [[lecture-bs-3]] (p17)
