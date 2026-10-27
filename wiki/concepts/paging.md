---
title: Paging
type: concept
tags: [memory, paging, allocation]
chapter: "04"
sources: [lecture-bs-4]
created: 2026-09-30
updated: 2026-09-30
---

# Paging

## Definition

**Paging** (Seitenadressierung) — *variant 2a* of memory allocation — abandons the requirement
that a process occupy a **contiguous** region of memory.

- Main memory is divided into equally sized **frames** (Seitenrahmen / Rahmen).
- Each process's address space is divided into equally sized **pages** (Seiten).
- **Page size = frame size.** Any page fits into any frame.

A per-process [[page-table]] records which frame currently holds which page.

## Context

Belongs to [[memory-management]]. It is the answer to the external [[fragmentation]] of
[[dynamic-partitioning]], and the foundation on which [[virtual-memory]] (variant 2b) is built:
virtual memory = paging **+ Present/Absent bit**.

## How it works

```
virtual address space          physical memory
+--------+                     +--------+ frame 0
| page 0 |-------------------->| page 2 |
+--------+          +--------->+--------+ frame 1
| page 1 |          |          |  ...   |
+--------+          |          +--------+ frame 2
| page 2 |----------+          | page 0 |
+--------+                     +--------+ frame 3
```

Consecutive pages need **not** land in consecutive frames. Translation is therefore a table
lookup rather than an addition — see [[address-translation]].

### Trade-offs

| | |
|---|---|
| (+) | **no external fragmentation** — every free frame is usable by every process |
| (+) | **little internal fragmentation** — at most the tail of the last page |
| (−) | **expensive address translation** — needs a [[memory-management-unit]] and a [[page-table]] per process |

## Related

- [[page-and-frame]] — the two units and why they must be equal in size.
- [[page-table]] — the mapping.
- [[virtual-memory]] — paging plus the Present bit.
- [[page-size]] — how the unit size is chosen.
- [[dynamic-partitioning]] — the scheme it replaces.

## Common confusions

- **Paging alone is not virtual memory.** Under plain paging *all* pages of a running process are
  resident; nothing is on disk, there are no page faults. Adding the Present/Absent bit is what
  makes it [[virtual-memory]].
- **Pages and frames are the same size — always.** This is not an implementation choice; it is
  what makes the offset pass through translation untouched.
- **Fragmentation is reduced, not eliminated.** See [[fragmentation]].

## Sources

- [[lecture-bs-4]] (p05, p06, p08)
