---
title: RAID 1 (Mirroring)
type: concept
tags: [file-system, storage, raid]
chapter: "05"
sources: [lecture-bs-5]
created: 2026-09-30
updated: 2026-09-30
---

# RAID 1 (Mirroring)

## Definition

**Mirroring**, for **failure safety**. At least **2** disks; **every block is stored on every disk
in the same stripe**. The disks are exact copies of one another.

```
disk 0:  A  B  C
disk 1:  A  B  C
```

## Read and write behaviour

- **Writes must go to all disks**, and are performed **in parallel**.
- **Reads can go to any disk** — so with two disks, **two independent reads can run in parallel**.

That asymmetry is the practical point: mirroring is bought for safety but also roughly doubles the
random **read** throughput, while leaving write throughput no better than a single disk.

## Pros and cons

| | |
|---|---|
| (+) | **the failure of one disk causes no data loss** |
| (+) | reads can be served by any disk, so read throughput scales |
| (−) | **capacity is only 50 %** (with two disks; 1/n in general) |
| (−) | **a write takes on average longer than with a single disk** |

The write penalty is a consequence of parallelism, not of extra work: the operation is finished
only when the **slowest** of the disks is finished, and the maximum of *n* independent latencies
exceeds the average of one.

## Context

Belongs to [[raid]]. It is the other endpoint of the line whose first endpoint is [[raid-0]] — all
safety, no capacity. [[raid-4]] and [[raid-5]] exist precisely to get most of RAID 1's safety at a
fraction of its capacity cost, using [[parity-and-xor|parity]] instead of full copies.

## Related

- [[raid-0]] — the opposite trade.
- [[nested-raid]] — RAID 0+1 and 1+0 combine the two.
- [[raid-4]], [[raid-5]] — the same protection for (n−1)/n of the capacity.
- [[raid-consistent-update-problem]] — the "write to all disks" step is exactly where a power
  failure is dangerous.
- [[hot-spare-and-hot-swap]] — how the failed disk is replaced afterwards.

## Common confusions

- **RAID 1 does not tolerate *any* failure.** It tolerates *disk* failures. A block corrupted by
  software is mirrored just as faithfully as a correct one.

## Sources

- [[lecture-bs-5]] (p22)
