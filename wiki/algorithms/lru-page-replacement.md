---
title: LRU (Least Recently Used)
type: algorithm
tags: [memory, virtual-memory, page-replacement]
chapter: "04"
sources: [lecture-bs-4]
created: 2026-09-30
updated: 2026-09-30
---

# LRU (Least Recently Used)

## Purpose

Choose the victim for [[page-replacement]]: **replace the page that has been unused the
longest**. The bet is [[locality]] run backwards — a page not touched for a long time is the one
least likely to be touched soon.

## Procedure

1. On every access to a page, record that it is now the most recently used.
2. On a [[page-fault]] with no free frame, evict the page whose last access lies furthest in the
   past.
3. If that page is a [[dirty-page]], write it back first.

### Naive implementation

A **linked list** of pages:

- **most recently used at the front**,
- **the victim at the back**.

Every access moves the touched page to the front; eviction takes the tail.

The implementation is what makes LRU expensive: the list must be updated on **every single memory
access**, not just on faults. That is impossible in software, so LRU **needs hardware support**
(a counter or a timestamp per frame).

## Worked example

Reference string `1 2 3 4 1 2 5 1 2 3 4 5` with **4 frames**.

| Ref | Frames (MRU → LRU) | Fault? |
|---|---|---|
| 1 | 1 | yes |
| 2 | 2, 1 | yes |
| 3 | 3, 2, 1 | yes |
| 4 | 4, 3, 2, 1 | yes |
| 1 | 1, 4, 3, 2 | no |
| 2 | 2, 1, 4, 3 | no |
| 5 | 5, 2, 1, 4 | yes — evicts 3 |
| 1 | 1, 5, 2, 4 | no |
| 2 | 2, 1, 5, 4 | no |
| 3 | 3, 2, 1, 5 | yes — evicts 4 |
| 4 | 4, 3, 2, 1 | yes — evicts 5 |
| 5 | 5, 4, 3, 2 | yes — evicts 1 |

8 page faults.

> [!question] Unverified — the trace above is reconstructed from a partly legible sketch in the
> notes (p14). The algorithm and the reference string are certain; the fault count is a
> reconstruction.

### Exercise 4-6 (p24)

Four frames, page table:

| Page | Frame | loaded at | last access | R | M |
|---|---|---|---|---|---|
| 0 | 2 | 26 | 162 | 1 | 0 |
| 1 | 1 | 130 | 160 | 0 | 0 |
| 2 | 0 | 60 | 161 | 0 | 1 |
| 3 | 3 | 20 | 163 | 1 | 1 |

**LRU replaces page 1** — its last access (160) is the oldest. Note that "loaded at" is irrelevant
to LRU; that column is what **FIFO** would use, and it would pick page 3 instead.

## Complexity / cost

- Decision: O(1) with a list, but the **bookkeeping is per access**, not per fault.
- Requires hardware support (counters/timestamps); a pure software implementation would multiply
  the cost of every memory reference.

## Pros and cons

| | |
|---|---|
| (+) | **works very well** — close to [[optimal-page-replacement|optimal]] in practice |
| (+) | never suffers Belady's anomaly |
| (−) | **costly**; needs hardware support |
| (−) | exact LRU is not implemented in real MMUs — hence the approximations |

## Compared to

| | Information | Cost | Picks (p24) |
|---|---|---|---|
| [[optimal-page-replacement]] | the future | impossible | page 3 |
| **LRU** | last-access time of every page | high | page 1 |
| [[clock-page-replacement]] | one Use bit + a hand | low | page 2 |
| [[not-recently-used]] | R and M bits | very low | page 1 |

LRU is also the policy used for **[[translation-lookaside-buffer|TLB]] entry replacement** (p11):
the entry unused the longest is the one replaced.

## Sources

- [[lecture-bs-4]] (p11, p14, p24)
