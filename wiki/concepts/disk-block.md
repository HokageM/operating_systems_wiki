---
title: Disk Block
type: concept
tags: [file-system, storage]
chapter: "05"
sources: [lecture-bs-5]
created: 2026-09-30
updated: 2026-09-30
---

# Disk Block

## Definition

Disks are divided into [[partition|partitions]] and, within those, into **blocks**. A **block is
the access unit for I/O** — the smallest amount of data that can be read from or written to the
medium.

## The size trade-off

| Large blocks | Small blocks |
|---|---|
| **good for sequential processing** — fewer requests, positioning cost amortised | more requests, more positioning |
| with **random access they violate the [[locality]] principle**: most of the block fetched is never used | little waste per access |
| more **[[fragmentation\|internal fragmentation]]** in the last block of every file | little internal fragmentation |
| fewer entries in the allocation structures | larger [[file-allocation-table\|FAT]] / more [[i-node]] pointers |

This is exactly the [[page-size]] trade-off of chapter 04, transposed to disk — including the
mechanism (internal fragmentation in the last unit) and the mitigating property (locality).

## Management of persistent storage — the tasks

The three duties named on p16:

1. **manage the disk blocks and their membership to a file**
2. **assign space to files**
3. **manage occupied and free storage**

Duty 1 is what the allocation methods answer ([[contiguous-allocation]],
[[linked-list-allocation]], [[file-allocation-table]], [[i-node]]).

## Worked example

Exercise S-3 c) (p32): how many **6 KiB** files fit into a **2 GiB** partition with **4 KiB**
blocks?

A 6 KiB file needs **two** blocks → it occupies **8 KiB**, of which 2 KiB are wasted —
**internal fragmentation**. Therefore

```
2 GiB / 8 KiB = 2^31 / 2^13 = 2^18 = 262 144 files
```

Not 2 GiB / 6 KiB. The whole point of the exercise is that you must round the file size *up to a
multiple of the block size* first.

## Related

- [[page-and-frame]] — the same fixed-size-unit idea in memory.
- [[page-size]] — the same trade-off, same shape.
- [[fragmentation]] — internal fragmentation in the last block; external fragmentation as
  [[disk-fragmentation]].
- [[locality]] — what large blocks exploit and, under random access, violate.
- [[file-system-architecture]] — "management of data blocks" is the job of the *simple file system*
  layer.
- [[solid-state-disk]] — enforces whole-block writes in hardware.

## Sources

- [[lecture-bs-5]] (p16, exercise S-3 c on p32)
