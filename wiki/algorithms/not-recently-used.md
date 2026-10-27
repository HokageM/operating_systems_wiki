---
title: NRU (Not Recently Used)
type: algorithm
tags: [memory, virtual-memory, page-replacement]
chapter: "04"
sources: [lecture-bs-4]
created: 2026-09-30
updated: 2026-09-30
---

# NRU (Not Recently Used)

## Purpose

Choose a [[page-replacement]] victim using only the two status bits that the hardware maintains
anyway — the **R bit** and the **M bit** of the [[page-table-entry]] — by sorting all pages into
four classes and evicting from the lowest non-empty one.

## Procedure

### The bits

- **R (Referenced)** — set by the hardware whenever the page is **accessed** (read or written).
- **M (Modified)** — set by the hardware whenever the page is **written to**.

Maintenance:

- Both bits are **reset for all pages of a process at process start**.
- The operating system **periodically resets R for all pages**. This is what makes R mean "used
  *recently*" rather than "ever used" — without the periodic sweep every bit would saturate at 1.
- M is **not** reset periodically; it is cleared only when the page is written back.

### The classes

| Class | R | M | Meaning |
|---|---|---|---|
| **0** | 0 | 0 | not recently used, not modified — **the ideal victim** |
| **1** | 0 | 1 | not recently used, but modified — needs a write-back |
| **2** | 1 | 0 | recently used, not modified |
| **3** | 1 | 1 | recently used and modified — **keep** |

1. Determine the **lowest non-empty class**.
2. Choose a page from it **at random**.

Class 1 ranking above class 2 is deliberate: NRU prefers a page that will *probably not be needed
again* even though evicting it costs a write-back, over a page that is *likely to be needed again
immediately* but is cheap to drop. Recency beats eviction cost.

## Worked example

Exercise 4-6 (p24), four frames:

| Page | Frame | loaded at | last access | R | M | Class |
|---|---|---|---|---|---|---|
| 0 | 2 | 26 | 162 | 1 | 0 | 2 |
| 1 | 1 | 130 | 160 | 0 | 0 | **0** |
| 2 | 0 | 60 | 161 | 0 | 1 | 1 |
| 3 | 3 | 20 | 163 | 1 | 1 | 3 |

Class 0 contains only page 1 → **NRU replaces page 1**. The random choice never comes into play
here, because the lowest non-empty class is a singleton.

## Complexity / cost

- Two bits per page, both maintained by hardware.
- One periodic sweep over the page table to clear R.
- The decision itself is a scan over the pages, classifying each in O(1).

## Pros and cons

| | |
|---|---|
| (+) | **simple and efficient** — the lecture's exact wording |
| (+) | takes the write-back cost of a [[dirty-page]] into account, which Clock does not |
| (−) | **not optimal**, but **acceptable** |
| (−) | within a class the choice is arbitrary, so it can evict a page that was about to be used |

## Compared to

| | Bits used | Victim in exercise 4-6 |
|---|---|---|
| [[lru-page-replacement]] | full access timestamps | page 1 |
| [[clock-page-replacement]] | R only (as the Use bit) | page 2 |
| **NRU** | R **and** M | page 1 |
| [[optimal-page-replacement]] | the future | page 3 |

NRU and [[clock-page-replacement|Clock]] are siblings: both approximate
[[lru-page-replacement|LRU]] with reference bits. Clock sweeps a pointer and clears bits as it
goes; NRU classifies the whole set at once and lets the OS clear R on a timer.

## Sources

- [[lecture-bs-4]] (p16, p24)
