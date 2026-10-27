---
title: Local and Global Page Replacement
type: concept
tags: [memory, virtual-memory, paging]
chapter: "04"
sources: [lecture-bs-4]
created: 2026-09-30
updated: 2026-09-30
---

# Local and Global Page Replacement

## Definition

The **scope** from which a [[page-replacement]] algorithm may choose its victim:

- **Local page replacement** — only the **faulting process's own pages** are considered. Each
  process keeps a **fixed number of frames**.
- **Global page replacement** — **all pages in main memory** are considered, including those
  belonging to other processes. Frames are allocated **dynamically**.

## Context

Belongs to [[virtual-memory]]. This is a second, independent axis to the choice of algorithm: any
of [[lru-page-replacement|LRU]], [[clock-page-replacement|Clock]] or [[not-recently-used|NRU]]
can be run locally or globally.

## How it works

| | Local | Global |
|---|---|---|
| Frames per process | fixed | varies over time |
| Isolation | a badly-behaved process only harms itself | one process can steal another's frames |
| Adaptation | poor — cannot follow a growing [[working-set]] | good — memory flows to where it is needed |
| Fairness | by construction | requires extra control |
| Thrashing | confined to one process | can spread across the system |

Global replacement is the more efficient default but needs a governor to keep one process from
starving the others — which is exactly what [[page-fault-frequency]] provides, by adding frames
to processes that fault too often and taking them from processes that fault too rarely.

## Related

- [[page-replacement]] — the procedure whose scope this fixes.
- [[page-fault-frequency]] — the control loop that makes global allocation safe.
- [[working-set]] — what the allocation should track.
- [[starvation]] — the risk global replacement introduces.

## Common confusions

- **Local does not mean "each process has enough".** It means "each process has a fixed amount" —
  which may be too little, in which case that process [[thrashing|thrashes]] on its own.
- **This is orthogonal to the algorithm.** "LRU or global?" is not a meaningful alternative; the
  answer is one of each.

## Sources

- [[lecture-bs-4]] (p17)
