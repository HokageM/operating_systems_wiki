---
title: File Systems
type: topic
tags: [file-system, storage, persistence]
chapter: "05"
sources: [lecture-bs-5]
created: 2026-09-30
updated: 2026-09-30
---

# File Systems

## Summary

A **file system** is the layer that turns a persistent storage device — a bag of numbered blocks
with no structure at all — into named, growable, hierarchically organised **[[file|files]]**. It
is the last of the four resource-management subjects of the module (after the CPU in
[[scheduling]], memory in [[memory-management]] and synchronisation in
[[concurrency-and-synchronisation]]), and it is by far the largest single block of chapter 05.

The chapter runs top-down and then bottom-up:

1. **The medium** — [[volatile-and-persistent-memory]], [[solid-state-disk]], [[wear-levelling]].
2. **The abstraction** — [[file]], [[file-management-system]], [[file-structures]],
   [[file-system-architecture]].
3. **Access** — [[file-access-types]], [[asynchronous-file-access]], [[memory-mapped-file]].
4. **Organisation** — [[file-metadata]], [[directory]], [[mounting]],
   [[logical-volume-manager]], [[partition]], [[disk-block]], [[disk-fragmentation]].
5. **Allocation on disk** — [[contiguous-allocation]], [[linked-list-allocation]],
   [[file-allocation-table]], [[i-node]], plus [[sparse-file]] and
   [[journaling-file-system]].
6. **Redundancy** — [[raid]] and its levels.

## Why it exists

Main memory is [[volatile-and-persistent-memory|volatile]]: everything in it disappears when the
power goes. Anything that must survive a reboot has to live on a persistent medium, and that medium
speaks in fixed-size blocks at a physical address, not in names and byte offsets.

The stated **goal of the [[file-management-system]] is transparency**: the application should not
have to know how or where storage actually works. That is the same promise the
[[memory-management-unit]] makes for memory, one level down and with a different unit — and it is
the reason the two subjects rhyme throughout.

## Key concepts

The five **criteria for file organisation and access** (p12) are the value system of the whole
chapter, and every design decision below is a trade among them:

1. **fast access**
2. **easy updating**
3. **economical use of storage**
4. **simple administration**
5. **reliability**

[[contiguous-allocation]] wins on 1 and loses on 2; [[linked-list-allocation]] wins on 2 and loses
on 1; [[i-node|i-nodes]] compromise. [[raid]] buys 5 (and sometimes 1) with 3.

## Mechanisms / algorithms

### The allocation methods, as a progression

```
contiguous            fast, but the size is fixed at creation and it fragments
   |  make the blocks chainable
   v
linked list           grows freely, but random access is O(n) and a lost pointer is fatal
   |  move the chain out of the data blocks into one table
   v
FAT                   full blocks for data, fast random access - but the whole table must be resident
   |  give every file its own small table instead of one global one
   v
i-node                only open files' tables are resident; direct/indirect levels scale with size
```

### The RAID levels

| Level | Idea | Buys | Costs | Net capacity |
|---|---|---|---|---|
| [[raid-0]] | striping | **speed** (≈ n×) | reliability falls | n/n |
| [[raid-1]] | mirroring | **reliability** | half the capacity | 1/n |
| [[raid-4]] | dedicated parity disk | 1-disk tolerance | parity disk is a bottleneck | (n−1)/n |
| [[raid-5]] | parity distributed over all disks | 1-disk tolerance + parallel writes | small-write penalty | (n−1)/n |
| [[raid-6]] | doubled, different parity | **2-disk** tolerance | worse writes, complex controller | (n−2)/n |
| [[nested-raid]] | 0+1 and 1+0 | speed **and** reliability | half the capacity | 1/2 |

Supporting machinery: [[parity-and-xor]], [[raid-parity-update]],
[[raid-consistent-update-problem]], [[hot-spare-and-hot-swap]].

## Trade-offs

- **Block size.** Large blocks are good for sequential processing but, with random access, violate
  the [[locality]] principle and waste space through [[fragmentation|internal fragmentation]].
  Exactly the [[page-size]] trade-off, on disk. See [[disk-block]].
- **Speed vs. safety.** Every RAID level is a point on this line; [[raid-0]] and [[raid-1]] are the
  two endpoints.
- **Consistency vs. write performance.** A [[journaling-file-system]] survives an aborted write, at
  the price of writing everything twice. A [[raid-consistent-update-problem|write-ahead log]] makes
  the same bargain inside the RAID controller.
- **Transparency vs. control.** [[mounting]] hides which device a file is on; [[partition|drive
  letters]] show it. Both appear in the notes as the two named alternatives.

## Exam relevance

Very high. The chapter supplies three calculation exercises, all of which are worth reproducing:

- **S-3** — [[i-node]] sizing: maximum file size, blocks needed for the administration structures,
  and how many 6 KiB files fit into a 2 GiB partition. The last part is a
  [[fragmentation|internal fragmentation]] question in disguise.
- **S-4** — [[disk-access-time]]: sequential vs. random effective transfer rate and IOPS from seek
  time, rotational delay and transfer rate. The punchline (47.6 MiB/s vs. 0.96 MiB/s) is the
  quantitative case for [[locality]].
- **S-5** and **S-6** — [[raid-0]]/[[raid-5]] address mapping, and the two methods of
  [[raid-parity-update]].

Plus the standing definition questions: file, directory, partition, volatile vs. persistent,
journaling, sparse file, hot-spare vs. hot-swap, and the advantages and disadvantages of each
allocation method and RAID level.

## Open questions

> [!question] Unverified — not in the notes
> **Disk-arm scheduling** (FCFS, SSTF, SCAN/elevator, C-SCAN) never appears, although S-4
> quantifies exactly the seek times such algorithms exist to minimise. Given that the notes cover
> CPU scheduling in the same lecture, its absence is conspicuous — worth checking against the
> official slides.

> [!question] Unverified — not in the notes
> Free-space management (bitmaps vs. free lists), directory implementation (linear lists vs.
> hashing), hard and symbolic links, and access rights beyond the bare mention in
> [[file-metadata]] are not covered.

## Sources

- [[lecture-bs-5]] — lecture part p10–p27, exercise recap S-3 to S-6.
