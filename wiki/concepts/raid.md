---
title: RAID
type: concept
tags: [file-system, storage, reliability, raid]
chapter: "05"
sources: [lecture-bs-5]
created: 2026-09-30
updated: 2026-09-30
---

# RAID

## Definition

**RAID** — **Redundant Array of Independent Disks**. Several disks are combined into a **logical
association** which offers **the same API as a single disk** and is therefore **fully
transparent** to everything above it. The data is **distributed** over the disks and stored
**redundantly**.

Two goals, and every level picks a point between them:

- **increased access speed** — parallel reading and writing;
- **increased reliability** — the array survives the failure of a disk.

## Procedure

> A **logical I/O request is decomposed into physical I/O requests**, whose answers together form
> the **logical answer**.

That one sentence is the whole mechanism, and it is worth reproducing verbatim: the controller is a
translator between one logical address space and *n* physical ones. The mappings are worked out in
[[raid-0]] and [[raid-5]].

## Hardware vs. software RAID

| | Hardware RAID controller | Software RAID controller |
|---|---|---|
| what it is | a **hardware component** | runs on the **host CPU** |
| CPU load | **none** | **high** |
| cost | expensive | **cost-effective** |
| booting | **can boot an operating system directly from the RAID** | cannot, in general — the OS must already be running |

## The levels

| Level | Idea | Tolerates | Net capacity | Page |
|---|---|---|---|---|
| **0** | striping | **0** failures | n/n | [[raid-0]] |
| **1** | mirroring | n−1 failures | 1/n | [[raid-1]] |
| **0+1 / 1+0** | nested | depends | 1/2 | [[nested-raid]] |
| **4** | dedicated parity disk | 1 | (n−1)/n | [[raid-4]] |
| **5** | distributed parity | 1 | (n−1)/n | [[raid-5]] |
| **6** | doubled parity | **2** | (n−2)/n | [[raid-6]] |

Note that **RAID 0 is not redundant at all** despite the R in the name — it makes reliability
*worse*.

## Context

Belongs to [[file-systems]]. It is the reliability answer of the chapter, as
[[journaling-file-system|journalling]] is the consistency answer. The two are independent and
complementary: RAID survives a *disk* failing, journalling survives a *write* being interrupted.

## Related

- [[parity-and-xor]] — the arithmetic behind levels 4, 5 and 6.
- [[raid-parity-update]] — what a single small write actually costs.
- [[raid-consistent-update-problem]] — why a RAID needs a write-ahead log.
- [[hot-spare-and-hot-swap]] — how a failed disk is replaced.
- [[logical-volume-manager]] — also combines disks into one logical device, but without redundancy.
- [[virtual-machine]] — "the same API as a single disk" is the definition of an abstraction of the
  real hardware; RAID is a virtual disk.
- [[direct-memory-access]] — the transfers the controller issues.

## Common confusions

- **RAID is not a backup.** It protects against a *disk* failing, not against deletion, corruption
  or fire. Every write, including a mistaken one, is faithfully replicated.
- **"Independent", not "inexpensive"** — the notes use the modern expansion.

## Sources

- [[lecture-bs-5]] (p20)
