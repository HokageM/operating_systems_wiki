---
title: Deadlock Detection Marking Algorithm
type: algorithm
tags: [deadlock, detection, algorithm, matrices]
chapter: "03"
sources: [lecture-bs-3]
created: 2026-09-30
updated: 2026-09-30
---

# Deadlock Detection — Marking Algorithm (Markierungsalgorithmus)

## Purpose

Decide whether a [[deadlock]] exists in a system with **multiple instances per resource**, where
a cycle in the [[resource-allocation-graph]] is only *necessary* and no longer sufficient — and
if one exists, **name the deadlocked processes**.

The same procedure, applied to a hypothetical state, is the safety test of the
[[bankers-algorithm]].

## Idea

Optimistic simulation: repeatedly look for a process whose **outstanding need** fits into what is
currently **free**. Such a process could run to completion, so pretend it does and give its
resources back. If every process can be disposed of this way, no deadlock exists. Whatever is
left unmarked is stuck.

## Procedure

### The four structures

For *n* processes and *m* resource types:

| Symbol | Name | Shape | Meaning |
|---|---|---|---|
| **R** | resource vector | 1 × m | total instances of each resource in the system |
| **Z** | allocation matrix (Belegungsmatrix) | n × m | what each process currently **holds** |
| **A** | request matrix (Anforderungsmatrix) | n × m | each process's **total claim** |
| **V** | availability vector | 1 × m | currently **free** |

with

```
V     = R − Σ(rows of Z)        free = total − allocated
A − Z = outstanding need         what each process still requires
```

### The four steps

1. **Unmark** all processes. Mark every process whose row in **Z** is all zeros (it holds nothing
   and therefore cannot be part of a deadlock).
2. Initialise a working vector **W := V**.
3. Find an **unmarked** process *i* whose outstanding need **(A − Z)ᵢ ≤ W** componentwise.
   - If none exists, **stop**.
   - Otherwise **mark** *i*, set **W := W + Zᵢ** (it finishes and returns its resources), and
     repeat step 3.
4. **All processes marked** ⟹ **no deadlock** (the state is [[safe-state|safe]]).
   **Some process unmarked** ⟹ **deadlock**, and the unmarked processes are exactly the
   deadlocked set.

## Worked example 1 — no deadlock (exercise 3-5, state 1)

Resources A B C D, `R = (10 9 8 7)`, five processes.

```
      A B C D                  A B C D
Z =   1 0 0 1           A =    3 4 3 2
      3 3 3 3                  8 3 7 7
      0 2 2 1                  5 5 5 4
      0 0 1 0                  3 3 2 1
      3 1 0 1                  4 1 2 2

ΣZ = (7 6 6 6)          V = R − ΣZ = (3 3 2 1)

            A B C D
A − Z =     2 4 3 1
            5 0 4 4
            5 3 3 3
            3 3 1 1
            1 0 2 1
```

Marking run:

| Step | W before | Process marked | need (A−Z) | + Z row | W after |
|---|---|---|---|---|---|
| 1 | 3 3 2 1 | **P5** | 1 0 2 1 ≤ W ✓ | 3 1 0 1 | 6 4 2 2 |
| 2 | 6 4 2 2 | **P4** | 3 3 1 1 ≤ W ✓ | 0 0 1 0 | 6 4 3 2 |
| 3 | 6 4 3 2 | **P1** | 2 4 3 1 ≤ W ✓ | 1 0 0 1 | 7 4 3 3 |
| 4 | 7 4 3 3 | **P3** | 5 3 3 3 ≤ W ✓ | 0 2 2 1 | 7 6 5 4 |
| 5 | 7 6 5 4 | **P2** | 5 0 4 4 ≤ W ✓ | 3 3 3 3 | 10 9 8 7 |

All five marked, and W has returned to **R = (10 9 8 7)**.
⟹ **kein Deadlock** — no deadlock. The state is [[safe-state|safe]], with safe sequence
P5, P4, P1, P3, P2.

## Worked example 2 — deadlock (exercise 3-5, state 2)

Resources A B C D, `R = (7 5 9 6)`, four processes.

```
      A B C D                  A B C D
Z =   2 0 3 1           A =    6 3 4 1
      1 1 1 1                  2 2 1 3
      0 0 5 1                  7 3 6 6
      1 2 0 0                  7 3 1 6

ΣZ = (4 3 9 3)          V = R − ΣZ = (3 2 0 3)

            A B C D
A − Z =     4 3 1 0
            1 1 0 2
            7 3 1 5
            6 1 1 6
```

Marking run:

| Step | W before | Process marked | need (A−Z) | + Z row | W after |
|---|---|---|---|---|---|
| 1 | 3 2 0 3 | **P2** | 1 1 0 2 ≤ W ✓ | 1 1 1 1 | 4 3 1 4 |
| 2 | 4 3 1 4 | **P1** | 4 3 1 0 ≤ W ✓ | 2 0 3 1 | 6 3 4 5 |
| 3 | 6 3 4 5 | — | P3 needs 7 3 1 5 ✗, P4 needs 6 1 1 6 ✗ | — | stop |

**P3 and P4 remain unmarked** (P3 needs 7 units of A but only 6 are available; P4 needs 6 units of
D but only 5 are available).
⟹ **Deadlock**, and the deadlocked set is **{P3, P4}**. The notes phrase this as
*"Deadlock unvermeidbar"* — the deadlock cannot be avoided.

## Complexity / cost

- **O(n² · m)** in the worst case: up to *n* passes, each scanning *n* processes over *m*
  resources.
- Must be **re-run** after each abort in successive [[deadlock-recovery]].
- Deciding **how often** to run it is the open practical question — see [[deadlock-detection]].

## Related

- [[deadlock-detection]] — the strategy this implements.
- [[resource-allocation-graph]] — the single-instance alternative.
- [[bankers-algorithm]] — same test, applied to a hypothetical future state.
- [[safe-state]] — what "all marked" means.
- [[deadlock-recovery]] — what follows a positive result.
- [[consumable-and-reusable-resources]] — why the multi-instance model exists.

## Sources

- [[lecture-bs-3]] (p18, p19, p27, p28)
