---
title: Logical Volume Manager (LVM)
type: concept
tags: [file-system, storage]
chapter: "05"
sources: [lecture-bs-5]
created: 2026-09-30
updated: 2026-09-30
---

# Logical Volume Manager (LVM)

## Definition

The **abstracted and dynamic management of file storages**: **logical volumes** which may
**extend over several physical drives** are presented to the file system as single storage areas.

## Pros and cons

| | |
|---|---|
| (+) | volumes can be **enlarged, created and shrunk during running operation** |
| (+) | the **capacity of several disks is combined** into one volume |
| (+) | **high data throughput** — accesses spread over several drives |
| (−) | the **failure of one disk loses all data** of a volume spanning it |

The disadvantage is the same arithmetic as [[raid-0]]: combining *n* independent devices into one
logical device multiplies the failure rate by *n*, because any one failure destroys the whole. An
LVM is therefore normally layered **on top of** a redundant [[raid]] rather than on bare disks.

## Context

Belongs to [[file-systems]]. It sits between the physical drives and the
[[partition|partitions]]/file systems, and it is what makes a file system's capacity a *policy*
rather than a property fixed at installation time.

## Related

- [[partition]] — the static counterpart: a fixed logical part of one physical disk.
- [[mounting]] — unifies name spaces; LVM unifies storage.
- [[raid]] — also combines several disks into one logical device, but for speed and redundancy
  rather than for flexible sizing.
- [[dynamic-partitioning]] — the same shift from fixed to runtime-variable sizing, in memory
  management.
- [[relocation]] and [[address-translation]] — again an indirection layer that lets the physical
  location change while the logical view stays fixed.

## Common confusions

- **LVM is not RAID.** By default an LVM stores no redundancy at all; spanning disks makes
  reliability *worse*. Any redundancy must come from a RAID level underneath.

## Sources

- [[lecture-bs-5]] (p15)
