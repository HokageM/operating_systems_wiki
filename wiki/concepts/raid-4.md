---
title: RAID 4
type: concept
tags: [file-system, storage, raid]
chapter: "05"
sources: [lecture-bs-5]
created: 2026-09-30
updated: 2026-09-30
---

# RAID 4

## Definition

Redundancy via **[[parity-and-xor|parity information]] instead of full mirroring**: for every
stripe an **extra parity block** is stored on a **separate, dedicated disk**. The parity is
computed **block-wise with XOR**.

```
disk 0   disk 1   disk 2   disk 3 (parity)
  DA       DB       DC       PABC
  DD       DE       DF       PDEF
  DG       DH       DI       PGHI
```

## Pros and cons

| | |
|---|---|
| (+) | **compensates the failure of exactly one disk** |
| (+) | **net capacity (n−1)/n** — far better than [[raid-1]]'s 1/n |
| (+) | **reads run in parallel** on the n−1 data disks |
| (−) | **every write must also write the parity disk**, and only **sequentially** |
| (−) | → **the parity disk is a bottleneck** |

## The bottleneck

Every write anywhere in the array touches the one parity disk. Writes to different data disks,
which could otherwise proceed in parallel, are therefore **serialised** at the parity disk. The
array's write throughput is that of a single disk, no matter how many disks it has.

This single defect is the entire reason [[raid-5]] exists, and "distribute the parity so it stops
being a bottleneck" is the answer to the standard exam question.

## Context

Belongs to [[raid]]. It is the first level that gets [[raid-1]]'s protection at
[[raid-0]]'s capacity — the idea is right, the placement is wrong.

## Related

- [[parity-and-xor]] — the arithmetic.
- [[raid-parity-update]] — what one small write actually costs; the additive method needs to read
  all the other data blocks, the subtractive one only two.
- [[raid-5]] — the fix.
- [[raid-6]] — two parity blocks, two-disk tolerance.
- [[raid-consistent-update-problem]] — data and parity must be updated atomically.

## Common confusions

- **The parity disk is not "the backup disk".** No single disk holds a copy of anything. If the
  *parity* disk fails, no data is lost at all; if a *data* disk fails, its contents are
  reconstructed by XOR-ing the rest.

## Sources

- [[lecture-bs-5]] (p24)
