---
title: Linked List Allocation
type: concept
tags: [file-system, storage, allocation]
chapter: "05"
sources: [lecture-bs-5]
created: 2026-09-30
updated: 2026-09-30
---

# Linked List Allocation

## Definition

The second allocation method: the blocks of a [[file]] form a chain, and the **first word of each
block points to the next block**. The directory entry stores only the address of the first block.

## How it works

```
dir entry -> [ next=9 | data ] -> [ next=3 | data ] -> [ next=-1 | data ]
              block 5             block 9             block 3
```

Blocks may lie anywhere on the disk, so any free block can be used and the file can grow as long as
free blocks exist.

## Pros and cons

| | |
|---|---|
| (+) | **simple** |
| (+) | **no external fragmentation** — every free block is usable |
| (+) | **files can grow and shrink** freely |
| (−) | **random access is very slow** — reaching block *i* means reading blocks 0…*i*−1 |
| (−) | **violates [[locality]]**, therefore needs **periodic reorganisation** |
| (−) | **error-prone: a lost pointer loses the whole rest of the file** |

There is a fourth, quieter cost: the pointer sits *inside* the data block, so a block no longer
holds a power-of-two amount of data. A read of one "block" of file data may then straddle two
physical blocks — which is precisely what [[file-allocation-table|FAT]] fixes.

## Context

Belongs to [[file-systems]]. It trades [[contiguous-allocation]]'s speed for flexibility, and the
trade is too expensive: [[file-access-types|direct access]] becomes O(n), and on a mechanical disk
each step of that chase costs a full [[disk-access-time|seek plus rotation]].

The two remaining methods both fix this by **moving the chain out of the data blocks**:

- [[file-allocation-table]] — one table for the whole medium, kept in main memory.
- [[i-node]] — one small table per file, loaded only while the file is open.

## Related

- [[contiguous-allocation]] — the predecessor.
- [[file-allocation-table]] — the direct improvement.
- [[i-node]] — the improvement that scales.
- [[disk-fragmentation]] — the reason for the "periodic reorganisation".
- [[pipe]] — the same ring-of-blocks idea used for communication rather than storage.

## Sources

- [[lecture-bs-5]] (p17)
