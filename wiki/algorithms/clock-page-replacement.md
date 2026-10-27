---
title: Clock Algorithm (Second Chance)
type: algorithm
tags: [memory, virtual-memory, page-replacement]
chapter: "04"
sources: [lecture-bs-4]
created: 2026-09-30
updated: 2026-09-30
---

# Clock Algorithm (Second Chance)

## Purpose

Approximate [[lru-page-replacement|LRU]] using only **one bit per frame**, so that the bookkeeping
can be done by hardware on every access at negligible cost.

## Procedure

The frames are arranged in a **cyclic list**. Each frame carries a **Use bit** (U bit; it is the
**R bit** of the [[page-table-entry]] — the notes say so explicitly in the margin of p24). A
**hand** points at one frame.

**On loading a page:**
1. Set its Use bit to **1**.
2. **Advance the hand** by one position.

**On a later reference to a resident page:**
- Set its Use bit to **1** (the hand does not move).

**On a [[page-fault]] with no free frame**, repeat until a victim is found:

1. Look at the frame under the hand.
2. If **U = 0** → **this is the victim**: swap the page out (writing it back first if it is a
   [[dirty-page]]), insert the new page, set its U bit, and **advance the hand**.
3. If **U = 1** → the page gets a *second chance*: **set U to 0** and **advance the hand**.
   Continue at step 1.

Because every pass clears a bit, the hand can sweep at most twice before it finds a zero, so the
loop always terminates.

## Worked example

```
            page 0 (U=1)
               |
   page 3 <----+----> page 1 (U=1)
   (U=1)       |
            page 2 (U=0)   <-- hand
```

The hand points at page 2 with U = 0 → **page 2 is replaced immediately**. This is exercise 4-6
(p24): the Clock algorithm picks **page 2**, where LRU and NRU both pick page 1 and the optimum
picks page 3.

If the hand had pointed at page 0 (U = 1), it would have cleared that bit, moved to page 1 (U = 1),
cleared it, and arrived at page 2 (U = 0) — the same victim, one sweep later.

## Complexity / cost

- **One bit per frame**, set by hardware on access.
- The decision is O(number of frames) in the worst case, O(1) amortised.
- No per-access software work at all — this is the whole point.

## Pros and cons

| | |
|---|---|
| (+) | very cheap: one bit and one pointer |
| (+) | a good approximation of LRU; recently used pages survive a sweep |
| (−) | coarser than LRU — it distinguishes only "used since the last sweep" from "not used" |
| (−) | the quality depends on how often the hand sweeps |

## Compared to

- [[lru-page-replacement]] — Clock is the cheap approximation of it; LRU knows the full ordering,
  Clock knows only one bit of recency.
- [[not-recently-used]] — also bit-based, but classifies **all** pages by (R, M) and picks
  randomly from the lowest class instead of sweeping a pointer. NRU additionally accounts for the
  **M bit**; plain Clock does not.
- [[optimal-page-replacement]] — the benchmark none of them reach.

The name *second chance* comes from step 3: a page with U = 1 is not evicted on sight but gets one
more sweep to prove it is still in use.

## Sources

- [[lecture-bs-4]] (p15, p24)
