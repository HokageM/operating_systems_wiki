---
title: Fragmentation
type: concept
tags: [memory, allocation]
chapter: "04"
sources: [lecture-bs-4, lecture-bs-5]
created: 2026-09-30
updated: 2026-09-30
---

# Fragmentation

## Definition

**Fragmentation** (Fragmentierung) is memory that is unusable even though it is not holding
anything. Two kinds are distinguished:

- **Internal fragmentation** (interne Fragmentierung) — unused memory **inside** an allocated
  area. It is assigned to a process but the process does not use it.
- **External fragmentation** (externe Fragmentierung) — unused memory **between** allocated
  areas. It is free, but the individual holes are too small to be useful.

## Context

Belongs to [[memory-management]]. Which kind you suffer from is determined entirely by the
allocation scheme, and the progression of the chapter is a progression through these two costs.

## How it works

| Scheme | Internal | External |
|---|---|---|
| [[static-partitioning]] | **yes** — a process rarely fills its fixed partition | yes, whole unused partitions |
| [[dynamic-partitioning]] | almost none — allocation is exact | **yes** — holes between areas |
| [[paging]] | **little** — only the last page of a process is partly unused | **none** — any free frame fits any page |

Paging's elimination of external fragmentation is the whole point of the scheme: because memory
need not be contiguous, *every* free frame is usable by *every* process.

The residual internal fragmentation under paging is on average half a page per process, which is
one of the arguments in the [[page-size]] trade-off.

## Related

- [[static-partitioning]] / [[dynamic-partitioning]] / [[paging]] — the three schemes above.
- [[page-size]] — larger pages mean more internal fragmentation.
- [[disk-fragmentation]] — external fragmentation on a file system.
- [[disk-block]] — internal fragmentation in the last block of every file; exercise S-3 c) turns this into a calculation.
- [[contiguous-allocation]] — fragments for exactly the reason [[dynamic-partitioning]] does.

## Common confusions

- **The mnemonic**: *in*ternal = *in*side an area; *ex*ternal = outside/between areas. Mixing the
  two up is the single most common error on this question, and it is asked directly (p18).
- **External fragmentation is not "memory being used by someone else".** It is genuinely free
  memory that simply cannot be allocated because it is not contiguous.
- **Paging does not remove fragmentation entirely** — it removes the *external* kind and reduces
  the *internal* kind to at most one partly-filled page per process.

## Sources

- [[lecture-bs-4]] (p04, p05, p18)
- [[lecture-bs-5]] (p16, p17, exercise S-3 c) — both kinds of fragmentation reappear on disk.
