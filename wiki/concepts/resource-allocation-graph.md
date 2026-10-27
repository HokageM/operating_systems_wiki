---
title: Resource Allocation Graph
type: concept
tags: [deadlock, graph, detection, resources]
chapter: "03"
sources: [lecture-bs-3]
created: 2026-09-30
updated: 2026-09-30
---

# Resource Allocation Graph (Betriebsmittel-Belegungsgraph)

## Definition

A **resource allocation graph** is a directed bipartite graph modelling the current allocation
state of a system:

- **Process nodes**, drawn as **circles** (P₁, P₂, …).
- **Resource nodes**, drawn as **boxes** (A, B, …); a box may contain one dot per **instance**.
- **Allocation edge** (Zuteilungskante) **resource → process**: this instance is currently held by
  that process.
- **Request edge** (Anforderungskante) **process → resource**: that process is waiting for this
  resource.

## Context

Belongs to [[deadlocks]]. It is the model used for
[[deadlock-detection]] when each [[consumable-and-reusable-resources|reusable resource]] has a
**single instance**; the multi-instance case needs the
[[deadlock-detection-marking-algorithm]] instead.

## How it works

### The cycle criterion

- **Single instance per resource:** a **cycle is necessary and sufficient** for a [[deadlock]].
  The processes on the cycle are exactly the deadlocked set.
- **Multiple instances per resource:** a cycle is only **necessary**. A cycle can exist while a
  free instance elsewhere lets some process finish and break it.

### Worked example (exercise 3-4)

Four processes P1…P4 issue request and release operations on resources A…F. Drawing the graph at
the critical moment and following the edges

```
A → P1 → B → P2 → E → P3 → A
```

gives a closed cycle. Each resource has a single instance, so this **is** a deadlock, and the
deadlocked set is **{P1, P2, P3}**. **P4 is not on the cycle** and is therefore not deadlocked,
even though it also waits for resources.

The exam skill is: build the graph from an operation trace, spot the cycle, and **name the
processes on it** — not merely say "there is a deadlock".

### When to check

The graph must be re-examined whenever the allocation state changes — i.e. on every request that
cannot be granted immediately. Checking on every request is exact but expensive; checking
periodically is cheap but leaves the deadlock standing for a while. The notes raise the question
without prescribing an answer.

## Related

- [[deadlock]] — the circular-wait condition made visual.
- [[deadlock-detection]] — the procedure this graph serves.
- [[deadlock-detection-marking-algorithm]] — the multi-instance alternative.
- [[consumable-and-reusable-resources]] — why single versus multiple instances matters.
- [[dining-philosophers]] — its graph is a five-cycle.

## Common confusions

- **A cycle is not always a deadlock.** Only with one instance per resource. This is the most
  commonly lost exam point.
- **Direction matters.** Resource → process means *held*; process → resource means *wanted*.
  Reversing them inverts the whole analysis.
- **Processes waiting outside the cycle are not deadlocked** — P4 in the example. They may be
  blocked *behind* the deadlock, but they are not part of it.

## Sources

- [[lecture-bs-3]] (p15, p18, p26)
