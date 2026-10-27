---
title: File Allocation Table (FAT)
type: concept
tags: [file-system, storage, allocation]
chapter: "05"
sources: [lecture-bs-5]
created: 2026-09-30
updated: 2026-09-30
---

# File Allocation Table (FAT)

## Definition

**Linked-list allocation with an index**: the third allocation method. The chain of
[[linked-list-allocation]] is kept not inside the data blocks but in a **separate table**, with one
entry per [[disk-block|block]] of the medium. The directory entry stores only the **address of the
index block** — the file's first block number — and the table supplies the rest of the chain.

## How it works

```
directory: "report.txt" -> first block 5

index table:   block:  3   5   9  ...
               next:  EOF  9   3
```

To reach logical block *i*, follow *i* entries **in the table in main memory** — no disk access at
all. Only the final block is actually read.

## Pros and cons

| | |
|---|---|
| (+) | the **whole data block is available for data** (no pointer stolen from it) |
| (+) | **good direct-access performance** — the chase happens in memory |
| (+) | the **directory entry only stores the address of the index block** |
| (−) | **the entire index of all files must be kept in main memory** |

The disadvantage is decisive and is what limits FAT to small media: the table has one entry per
block *of the whole medium*, resident whether the files are open or not. For a large disk with
small blocks this is hundreds of megabytes of RAM holding mostly entries for files nobody is using.

## Context

Belongs to [[file-systems]]. FAT and [[i-node|i-nodes]] are the two answers to the same question —
*where do we put the chain?* — and they differ in **granularity**:

| | Scope of the index | Resident when |
|---|---|---|
| **FAT** | one table for the **entire medium** | **always** |
| [[i-node]] | one small table **per file** | only while the file is **open** |

That difference is the entire reason UNIX file systems use i-nodes.

## Related

- [[linked-list-allocation]] — the method it improves.
- [[i-node]] — the alternative, per-file index.
- [[page-table]] — the same structural idea in memory management: a separate table mapping logical
  unit numbers to physical unit numbers. And [[multi-level-page-table]] is to [[page-table]] what
  [[i-node|i-nodes]] are to FAT — a way to stop keeping the whole index resident.
- [[cache]] — the resident table is effectively a permanently cached index.

## Sources

- [[lecture-bs-5]] (p18)
