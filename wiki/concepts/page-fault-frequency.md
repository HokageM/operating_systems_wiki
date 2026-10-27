---
title: Page Fault Frequency
type: concept
tags: [memory, virtual-memory, performance]
chapter: "04"
sources: [lecture-bs-4]
created: 2026-09-30
updated: 2026-09-30
---

# Page Fault Frequency

## Definition

**Page fault frequency** (Seitenfehlerrate) is used as a **control signal for frame allocation**:
a fault rate **between two bounds A and B is acceptable**.

- rate **above A** (too many faults) → **assign the process additional frames**
- rate **below B** (too few faults) → **take frames away** from the process

## Context

Belongs to [[virtual-memory]]. It is the feedback loop that makes
[[local-and-global-page-replacement|global replacement]] safe and that keeps each process's frame
allocation tracking its [[working-set]] without the OS having to measure that working set
directly.

## How it works

```
        faults/s
           ^
           |
   A  -----+---------------  above A: give this process more frames
           |    acceptable
   B  -----+---------------  below B: reclaim frames from this process
           |
           +----------------> frames assigned
```

The elegance is that the fault rate is a **directly observable proxy** for "does the working set
fit?" — the OS counts page faults it is handling anyway. No page-reference tracing is required.

The lower bound matters as much as the upper one: a process faulting far too rarely is holding
more frames than it needs, and those frames are better spent on a process that is close to
[[thrashing]].

## Related

- [[working-set]] — the quantity being tracked indirectly.
- [[thrashing]] — what the upper bound is there to prevent.
- [[local-and-global-page-replacement]] — the allocation policy this governs.
- [[hit-rate]] — the same measurement from the other side.

## Common confusions

- **This is an *allocation* mechanism, not a replacement algorithm.** It decides *how many* frames
  a process gets; [[lru-page-replacement|LRU]] and friends decide *which page* leaves.
- **Both bounds act.** Remembering only "many faults → more frames" loses half the mechanism.

## Sources

- [[lecture-bs-4]] (p17)
