---
title: Contiguous Allocation
type: concept
tags: [file-system, storage, allocation]
chapter: "05"
sources: [lecture-bs-5]
created: 2026-09-30
updated: 2026-09-30
---

# Contiguous Allocation

## Definition

The first of the four allocation methods: a [[file]] occupies a **sequence of contiguous
[[disk-block|blocks]]**. The directory entry needs only the **start block and the length**.

## How it works

```
file A: blocks 0 1 2 3          file B: blocks 7 8
        [####]    (free 4-6)            [##]
```

Finding logical block *i* is a single addition: `start + i`. Reading the file end to end requires
one positioning operation and then pure streaming.

## Pros and cons

| | |
|---|---|
| (+) | **simple** — two numbers per file |
| (+) | **very good read/write performance**, sequential *and* direct (see [[file-access-types]]) |
| (−) | the **maximum size is fixed at creation** — the file cannot grow beyond its neighbours |
| (−) | **fragmentation** (see [[disk-fragmentation]]) |
| (−) | **compaction is very expensive** — moving gigabytes to close gaps |

## Context

Belongs to [[file-systems]]. It is the disk counterpart of
[[dynamic-partitioning]] in memory management, and it inherits that scheme's problem exactly:
variable-size contiguous areas leave unusable gaps between them, and the only cure — compaction —
costs more than it saves. Chapter 04 solved this with [[paging]]; the file system solves it with
the block-chaining methods that follow.

## Related

- [[linked-list-allocation]] — the next step: give up contiguity to gain growth.
- [[file-allocation-table]], [[i-node]] — the practical descendants.
- [[dynamic-partitioning]] and [[paging]] — the same problem and the same solution one layer up.
- [[fragmentation]] — external fragmentation is its characteristic failure.
- [[disk-access-time]] — why contiguity is worth so much on a mechanical disk.

## Common confusions

- Contiguous allocation is not obsolete: it is still used where files are written once and never
  grow — optical media, and the "extent"-based allocation of modern file systems, which is
  contiguous allocation applied to *runs* of blocks rather than whole files.

## Sources

- [[lecture-bs-5]] (p17)
