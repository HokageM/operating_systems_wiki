---
title: Dynamic Partitioning
type: concept
tags: [memory, allocation]
chapter: "04"
sources: [lecture-bs-4]
created: 2026-09-30
updated: 2026-09-30
---

# Dynamic Partitioning

## Definition

**Dynamic partitioning** (dynamische Partitionierung) — *variant 2* of memory allocation — lets
the **number, start address and size** of the partitions **change at runtime**. Each process gets
a contiguous area of exactly the size it requested, carved out of main memory when it is loaded.

## Context

Belongs to [[memory-management]]. It fixes the two weaknesses of [[static-partitioning]] but
introduces external [[fragmentation]], which is in turn the motivation for [[paging]].

## How it works

### Trade-offs

| | |
|---|---|
| (+) | **exact allocation** — a process receives precisely as much memory as it asked for, so almost no internal fragmentation |
| (−) | the **exact need is not known in advance** — a process's memory demand changes at runtime |
| (−) | **more administration** — the OS must track a variable list of holes and occupied areas |
| (−) | **external fragmentation** — free memory ends up split into many unusable small holes |

### Handling a request for more memory at runtime

The lecture lists three answers:

1. **Occupy an adjacent hole** — grow into the free area directly behind the process, if one
   exists.
2. **Relocate other processes** — move neighbours aside to create a large enough hole; this
   requires [[relocation]] and is expensive.
3. **"Room to grow"** — allocate more than requested from the start, leaving slack behind the
   process. This trades external for internal fragmentation.

## Related

- [[static-partitioning]] — the fixed-size predecessor.
- [[paging]] — abandons contiguity altogether and thereby kills external fragmentation.
- [[fragmentation]] — external fragmentation is the defining cost here.
- [[relocation]] — what makes strategy 2 possible.

## Common confusions

- **Dynamic partitioning is still contiguous.** Each process occupies **one** connected block.
  That is precisely why holes between blocks cannot be used. Only [[paging]] drops the contiguity
  requirement.
- **"Exact allocation" does not mean "no waste".** The waste moves from *inside* the areas to
  *between* them.

## Sources

- [[lecture-bs-4]] (p05)
