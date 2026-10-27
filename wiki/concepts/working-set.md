---
title: Working Set
type: concept
tags: [memory, virtual-memory, performance]
chapter: "04"
sources: [lecture-bs-4]
created: 2026-09-30
updated: 2026-09-30
---

# Working Set

## Definition

The **working set** of a process is the **set of pages the process uses within a given time
interval**. It is a moving window over the process's reference string: the pages touched in the
last Δ references.

## Context

Belongs to [[virtual-memory]]. It is the operational form of [[locality]]: locality says accesses
cluster, the working set names *which* pages the current cluster consists of.

## How it works

The central statement of the lecture:

> If main memory cannot hold the working set, many [[page-fault|page faults]] occur.

This gives an allocation rule: **each process needs at least as many frames as its working set has
pages.** Below that threshold the process spends its time faulting rather than computing — see
[[thrashing]].

The working set is not constant. It changes as the process moves between phases (initialisation,
main loop, shutdown), so the frame allocation has to follow it. Two mechanisms in the chapter do
this:

- [[page-fault-frequency]] — measure the fault rate and add or remove frames accordingly.
- [[local-and-global-page-replacement|global replacement]] — let processes compete for frames
  dynamically.

It is also the natural heuristic for [[prepaging]]: when a process is resumed, load its working
set back in one batch.

## Related

- [[locality]] — the underlying property.
- [[thrashing]] — what happens when the working set does not fit.
- [[page-fault-frequency]] — how the OS estimates whether it fits.
- [[page-size]] — page size changes how many pages a given working set occupies.
- [[swapping]] — a swapped-out process loses its whole working set at once.

## Common confusions

- **The working set is defined over a time window, not over the whole run.** "All pages the
  process ever touches" is the process's address space, not its working set.
- **Working set ≠ resident set.** The working set is what the process *needs*; the resident set is
  what it has *got*. Thrashing is precisely the case where the second is smaller than the first.

## Sources

- [[lecture-bs-4]] (p13)
