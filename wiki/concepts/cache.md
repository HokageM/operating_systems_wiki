---
title: Cache
type: concept
tags: [memory, caching, hardware]
chapter: "04"
sources: [lecture-bs-4, lecture-bs-5]
created: 2026-09-30
updated: 2026-09-30
---

# Cache

## Definition

A **cache** is a **fast intermediate memory placed between main memory and the CPU**. It holds
copies of recently used data so that repeated accesses do not have to go to the slower level
below.

## Context

Belongs to [[memory-management]], and is the hardware-managed boundary of the
[[memory-hierarchy]]. It works only because of [[locality]]. Its analogue for address translation
is the [[translation-lookaside-buffer]].

## How it works

### Read access

1. **Hit** — the datum is in the cache → deliver it immediately.
2. **Miss** — the datum is not in the cache → load the **cache line** containing it from main
   memory into the cache, then deliver it.
3. If the cache is **full**, a **replacement criterion** (Verdrängungskriterium) decides which
   line is evicted.

Loading a whole *line* rather than a single word is spatial [[locality]] turned into profit.

### Write access

The datum is stored in the cache; the cache and main memory then have to be reconciled, by one of
two policies — see [[write-through-and-write-back]].

### Multi-level caches

Real machines stack several caches (L1, L2, L3), each larger and slower than the one above. A miss
in L1 is looked up in L2, a miss in L2 in L3, and only then in main memory.

## Related

- [[write-through-and-write-back]] — the two write policies.
- [[translation-lookaside-buffer]] — a cache specifically for page-number → frame-number mappings.
- [[memory-technologies]] — caches are built from SRAM.
- [[hit-rate]] — how the benefit of a cache is quantified.
- [[process-switch]] — a process switch invalidates/flushes cache contents, which is a large part
  of why it is expensive.
- [[file-system-architecture]] — the I/O supervisor's **I/O buffers** are a block cache.
- [[file-allocation-table]] — a permanently resident index, i.e. a cache that is never evicted.
- [[raid-consistent-update-problem]] — the battery-backed write-ahead log is physically a write-back cache.

## Common confusions

- **The cache is not addressed by the program.** It is transparent: the program issues ordinary
  memory addresses and the hardware decides whether the access is served from the cache.
- **Cache vs. TLB.** A cache holds *data*; the [[translation-lookaside-buffer]] holds *address
  translations*. A single memory access can hit in one and miss in the other.
- **Cache vs. buffer.** A cache exploits repeated access to the *same* data; a buffer smooths a
  rate mismatch between producer and consumer (see [[pipe]]).

## Sources

- [[lecture-bs-4]] (p02, p03, p21)
- [[lecture-bs-5]] (p12, p18, p23) — buffering and caching throughout the file system.
