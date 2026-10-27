---
title: Optimal Page Replacement
type: algorithm
tags: [memory, virtual-memory, page-replacement]
chapter: "04"
sources: [lecture-bs-4]
created: 2026-09-30
updated: 2026-09-30
---

# Optimal Page Replacement

## Purpose

The theoretical benchmark for [[page-replacement]]: **replace the page that will be needed
furthest in the future** (in the exercise's wording, the page that is "needed last"). It produces
the provably minimum number of [[page-fault|page faults]] for a given reference string.

## Procedure

1. On a page fault with no free frame, look ahead in the reference string.
2. For each resident page, find the time of its **next** use.
3. Evict the page whose next use is **latest** — or which is never used again.

## Worked example

Exercise 4-6 (p24), four frames. The exercise asks for the victim under the *ideal* strategy given
that the future reference order is known:

| Page | Frame | loaded at | last access | R | M |
|---|---|---|---|---|---|
| 0 | 2 | 26 | 162 | 1 | 0 |
| 1 | 1 | 130 | 160 | 0 | 0 |
| 2 | 0 | 60 | 161 | 0 | 1 |
| 3 | 3 | 20 | 163 | 1 | 1 |

**The optimum replaces page 3** — it is the page needed last. Note that this is the page every
practical algorithm *protects*: it has R = 1 and M = 1, so it sits in NRU's class 3 and has the
most recent access time of all. The heuristics are exactly wrong here, which is the point of the
exercise.

## Complexity / cost

**Not implementable.** It requires knowledge of the future reference string, which the OS does not
have. It is used only:

- as a **yardstick** when evaluating real algorithms in simulation, and
- as an exam question, to check that you understand what the heuristics are approximating.

## Pros and cons

| | |
|---|---|
| (+) | provably minimal number of page faults |
| (+) | immune to Belady's anomaly |
| (−) | **impossible to implement** — needs the future |

## Compared to

Every other algorithm in the chapter is an attempt to guess what the optimum knows:

- [[lru-page-replacement]] — assumes the **recent past mirrors the near future**. The best of the
  practical approximations.
- [[clock-page-replacement]] — approximates LRU with one bit.
- [[not-recently-used]] — approximates LRU with two bits, plus eviction cost.

In exercise 4-6 the four strategies give three different answers — page 1 (LRU, NRU), page 2
(Clock), page 3 (optimal) — which makes it an ideal revision exercise.

## Sources

- [[lecture-bs-4]] (p24)
