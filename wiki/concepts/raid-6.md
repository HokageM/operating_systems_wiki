---
title: RAID 6
type: concept
tags: [file-system, storage, raid]
chapter: "05"
sources: [lecture-bs-5]
created: 2026-09-30
updated: 2026-09-30
---

# RAID 6

## Definition

RAID 6 tolerates the **simultaneous failure of up to two disks**. It is striping with **doubled
parity** distributed at block level, computed by **two different mathematical computations**.

The notes describe the second computation concretely: besides the **XOR within a stripe**, there is
an **additional XOR over diagonal block widths**, so that

- **every data block is secured by two different parity blocks**, and
- **never may two data blocks flow together into the same two parity blocks**.

That last condition is the crucial one. If two data blocks appeared in the same pair of parity
blocks, losing exactly those two would leave two unknowns in two equations that are not
independent — and the pair would be unrecoverable. The diagonal construction guarantees
independence.

## Pros and cons

| | |
|---|---|
| (+) | **tolerates two simultaneous disk failures** |
| (−) | **write performance penalty greater than [[raid-5]]** — two parity blocks to update per write |
| (−) | **more complex controller** |
| — | **net capacity (n−2)/n** |

## Why two-disk tolerance is worth the cost

A [[raid-5]] array is unprotected from the moment one disk fails until the rebuild completes, and
the rebuild reads *every* remaining disk in full — the most stressful operation the array ever
performs, at the worst possible moment. As disks grow, the rebuild window grows with them, and a
second failure during it stops being unlikely. RAID 6 covers exactly that window.

## Context

Belongs to [[raid]]. It is the last level in the notes and the endpoint of the parity progression:
[[raid-4]] (parity, badly placed) → [[raid-5]] (parity, well placed) → RAID 6 (two independent
parities).

## Related

- [[parity-and-xor]] — the first of the two computations.
- [[raid-5]] — the baseline it extends.
- [[raid-parity-update]] — doubled, hence the larger write penalty.
- [[hot-spare-and-hot-swap]] — the other way to shorten the vulnerable window.

## Common confusions

- **"Two different computations" does not mean XOR twice over the same set.** Two identical parity
  blocks would carry no more information than one. The computations must be *independent*, which is
  what the diagonal grouping achieves.

## Sources

- [[lecture-bs-5]] (p26)
