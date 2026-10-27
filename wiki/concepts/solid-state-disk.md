---
title: Solid State Disk (SSD)
type: concept
tags: [file-system, storage, hardware]
chapter: "05"
sources: [lecture-bs-5]
created: 2026-09-30
updated: 2026-09-30
---

# Solid State Disk (SSD)

## Definition

A **non-volatile electronic storage medium** with **no mechanical components**
(see [[volatile-and-persistent-memory]]).

## Properties

Consequences of having no moving parts:

| | |
|---|---|
| (+) | **mechanically robust** |
| (+) | **short access times** — no seek, no rotational delay (contrast [[disk-access-time]]) |
| (+) | **no noise** |
| (−) | **blocks can only be read and written as a whole** |
| (−) | cells wear out with each write cycle → [[wear-levelling]] is required |

The "whole block only" restriction is the important one: an SSD cannot modify a byte in place, so
every small write becomes a read-modify-write of an entire block. That is the flash analogue of the
[[raid-parity-update|small-write penalty]] in RAID.

## Interfaces

| Medium | Interface |
|---|---|
| HDD (hard disk) | **SATA** |
| SSD | **NVMe** |

NVMe exists because SATA was designed around the latencies of a mechanical disk and became the
bottleneck once those latencies disappeared.

## Related

- [[volatile-and-persistent-memory]] — the class it belongs to.
- [[wear-levelling]] — the controller function that keeps it alive.
- [[disk-block]] — the unit it is forced to work in.
- [[disk-access-time]] — the mechanical costs it does *not* have; exercise S-4 quantifies the
  difference an SSD removes.
- [[disk-fragmentation]] — largely irrelevant on an SSD, because there is no arm to move.
- [[memory-hierarchy]] — the SSD sits between main memory and the mechanical disk.

## Common confusions

- **"Disk" is a misnomer.** There is no disk and nothing rotates; the name survives because the SSD
  presents the same block interface as a hard disk, which is precisely what makes it a drop-in
  replacement.
- **Defragmenting an SSD is pointless and harmful** — it buys no seek time (there is none) and
  consumes write cycles.

## Sources

- [[lecture-bs-5]] (p10)
