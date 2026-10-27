---
title: Prepaging
type: concept
tags: [memory, paging, virtual-memory]
chapter: "04"
sources: [lecture-bs-4]
created: 2026-09-30
updated: 2026-09-30
---

# Prepaging

## Definition

**Prepaging** (Vorausladen) is the *fetch policy* under which pages are loaded into main memory
**before** they are demanded, in the expectation that they will be needed soon.

## Context

Belongs to [[virtual-memory]], as the counterpart of [[demand-paging]].

## How it works

The lecture names two situations in which prepaging pays off:

1. **At process start.** Loading a batch of pages up front avoids the burst of page faults that
   pure [[demand-paging]] produces while a process warms up.
2. **When pages lie adjacent on persistent storage.** Reading several consecutive blocks in one
   disk operation is far cheaper than reading them one at a time, because the expensive part of a
   disk access is the positioning, not the transfer. This is spatial [[locality]] applied to the
   disk rather than to memory.

The risk is symmetric: pages loaded speculatively and never used waste both I/O bandwidth and —
more importantly — **frames**, which may force out pages that were still in use.

## Related

- [[demand-paging]] — the default policy.
- [[working-set]] — a good prepaging heuristic is "load the process's working set".
- [[locality]] — the justification.
- [[direct-memory-access]] — block transfers rest on the same argument.

## Common confusions

- **Prepaging is not caching ahead of the CPU.** It concerns whole pages moving between disk and
  main memory, not cache lines.
- **It does not replace demand paging**; real systems prepage a little and demand-page the rest.

## Sources

- [[lecture-bs-4]] (p14)
