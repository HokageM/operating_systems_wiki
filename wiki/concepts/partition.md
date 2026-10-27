---
title: Partition
type: concept
tags: [file-system, storage]
chapter: "05"
sources: [lecture-bs-5]
created: 2026-09-30
updated: 2026-09-30
---

# Partition

## Definition

A **partition** is a **logical part of a hard disk which is addressed like a physical one**.

The rule to remember: **every partition holds exactly one file system.**

## Advantages

| | |
|---|---|
| (+) | **individual reformatting** — one partition can be wiped without touching the others |
| (+) | **hardware error isolation** — a defect in one area affects only its partition |
| (+) | **an own operating system per partition** — the basis of multi-boot |
| (+) | **structuring by operating-system and user data** — the classic system/data split |

## Context

Belongs to [[file-systems]]. Partitioning is the **static, subtractive** way of organising
storage — one physical disk cut into several logical ones. A
[[logical-volume-manager|logical volume manager]] is the **dynamic, additive** counterpart, several
physical disks combined into one logical one. Both produce "a thing that is addressed like a
physical disk"; they differ in direction and in whether the sizes can change at runtime.

## Related

- [[logical-volume-manager]] — the dynamic alternative.
- [[mounting]] — how the file systems of several partitions become one name space.
- [[disk-block]] — "disks are divided into partitions and blocks": partitions first, then blocks
  within them.
- [[static-partitioning]] — the memory-management analogue, with the same advantages (simplicity,
  isolation) and the same disadvantage (a size fixed in advance, hence waste). The shared name is
  not a coincidence.
- [[fragmentation]] — space free in one partition cannot be used by another, which is external
  fragmentation at the partition level.

## Common confusions

- **Partition vs. file system.** The partition is the *container*; the file system is the
  *structure inside it*. "Formatting a partition" means creating a file system in it.
- **Partition vs. [[solid-state-disk]] block.** Partitioning is a logical division made by the
  operating system; block structure is imposed by the hardware.

## Sources

- [[lecture-bs-5]] (p15, p16)
