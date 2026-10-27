---
title: RAID 0 (Striping)
type: concept
tags: [file-system, storage, raid]
chapter: "05"
sources: [lecture-bs-5]
created: 2026-09-30
updated: 2026-09-30
---

# RAID 0 (Striping)

## Definition

**Striping**, for **acceleration only**. At least **2** disks are divided into contiguous
**stripes**; the data is split into equal-sized blocks and distributed over the disks in a
**zipper procedure** (Reißverschlussverfahren), so that consecutive logical blocks land on
different disks and can be accessed **in parallel**.

```
logical blocks:  0  1  2  3  4  5
disk 0:          0     3
disk 1:             1     4
disk 2:                2     5
```

## Address mapping

Exercise S-5 1) (p34), for *n* disks:

```
disk           = A mod n
block address  = A / n        (integer division)
```

where **A** is the logical block address. This is simply base-*n* decomposition: the remainder
selects the disk, the quotient the position on it.

## Pros and cons

| | |
|---|---|
| (+) | operations are **distributed over several disks** |
| (+) | **acceleration factor ≈ the number of disks** |
| (−) | **the failure of one disk loses practically all data** — every file is spread over all of them |
| (−) | **reliability sinks with the number of disks**: *n* disks fail *n* times as often |

The effective speed depends on the **access pattern and the block size**: a large sequential read
touches all disks at once and gets the full factor; a small random read touches one disk and gets
nothing (see [[disk-access-time]] and [[file-access-types]]).

## Use

**Pure speed for sequential throughput, where failure safety does not matter** — audio and video
work, temporary intermediate storage.

## Context

Belongs to [[raid]]. It is one endpoint of the speed/safety line, with [[raid-1]] at the other.
Its reliability arithmetic is identical to that of a spanning
[[logical-volume-manager|logical volume]]: combining *n* devices into one multiplies the failure
rate by *n*.

## Related

- [[raid]] — the general scheme.
- [[raid-1]] — the opposite trade.
- [[nested-raid]] — RAID 0+1 and 1+0 combine the two.
- [[raid-5]] — striping *plus* redundancy, which is what most people actually want.
- [[disk-access-time]] — why striping helps, and when it does not.

## Common confusions

- **RAID 0 is not redundant**, so the "R" is a misnomer for this level. It is the only RAID level
  that makes data *less* safe than a single disk.

## Sources

- [[lecture-bs-5]] (p21, exercise S-5 on p34)
