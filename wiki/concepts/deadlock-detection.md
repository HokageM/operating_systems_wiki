---
title: Deadlock Detection
type: concept
tags: [deadlock, detection, strategy]
chapter: "03"
sources: [lecture-bs-3]
created: 2026-09-30
updated: 2026-09-30
---

# Deadlock Detection (Deadlock-Erkennung)

## Definition

**Deadlock detection** allows deadlocks to occur, but **examines the allocation state
periodically** to determine whether a [[deadlock]] currently exists and **which processes are
involved**. It is always paired with [[deadlock-recovery]].

## Context

Belongs to [[deadlocks]]. It is the third of the four strategies and the one the chapter spends
most calculation effort on — both worked exercises (3-4 and 3-5) are detection problems.

## How it works

Which procedure applies depends on the resource model:

| Model | Procedure | Criterion |
|---|---|---|
| **Single instance** per resource | build the [[resource-allocation-graph]] | a **cycle** — necessary **and sufficient** |
| **Multiple instances** per resource | [[deadlock-detection-marking-algorithm]] | processes left **unmarked** at the end are deadlocked |

### The multi-instance model

Four structures describe the state, for *n* processes and *m* resource types:

| Symbol | Name | Shape | Meaning |
|---|---|---|---|
| **R** | resource vector (Betriebsmittelvektor) | 1 × m | total number of instances of each resource |
| **Z** | allocation matrix (Belegungsmatrix) | n × m | what each process currently **holds** |
| **A** | request matrix (Anforderungsmatrix) | n × m | what each process **will need in total** |
| **V** | availability vector (Verfügbarkeitsvektor) | 1 × m | what is currently **free** |

They are related by

```
V = R − Σ (rows of Z)          i.e.  free = total − allocated
A − Z                          = the outstanding need of each process
```

The marking algorithm then repeatedly asks whether any unfinished process's outstanding need fits
into the currently available vector — see [[deadlock-detection-marking-algorithm]] for the four
steps and both worked examples.

### When to run it

The notes pose the question without answering it definitively: checking on **every request that
cannot be granted** is exact but costly; checking **periodically** is cheap but lets the deadlock
persist. A practical compromise is to check when CPU utilisation drops while processes are
blocked.

## Related

- [[deadlock]] — what is being looked for.
- [[resource-allocation-graph]] — the single-instance procedure.
- [[deadlock-detection-marking-algorithm]] — the multi-instance procedure.
- [[deadlock-recovery]] — what must follow a positive detection.
- [[bankers-algorithm]] — uses the same marking test, but *hypothetically*, before granting.
- [[consumable-and-reusable-resources]] — why the two models exist.

## Common confusions

- **A and Z are not the same kind of quantity.** In these notes **A is the total claim**, not the
  outstanding request — the outstanding need is **A − Z**. Mixing them up produces the wrong
  answer on every exercise.
- **Detection alone does nothing.** It reports; [[deadlock-recovery]] acts.
- **Detection is not [[deadlock-avoidance]].** Detection runs *after* allocations, on the real
  state; avoidance runs *before* one, on a hypothetical state.

## Sources

- [[lecture-bs-3]] (p18, p19, p26, p27, p28)
