---
title: i-node
type: algorithm
tags: [file-system, storage, allocation]
chapter: "05"
sources: [lecture-bs-5]
created: 2026-09-30
updated: 2026-09-30
---

# i-node

## Purpose

**Allocation with index nodes**: the fourth and best allocation method. Each [[file]] gets its own
small index structure — the **i-node**, a **file control block** containing the file's
[[file-metadata|attributes]] plus a **table of block addresses**. Unlike a
[[file-allocation-table|FAT]], only the i-nodes of **open** files need to be in main memory.

## Procedure

### Structure

- The i-node holds the attributes and a **small table of block addresses**.
- The file's logical blocks are **numbered consecutively, starting with the directly addressed
  blocks**.
- Only a **few addresses are stored directly**; the rest are reached through **indirect blocks**:

| Pointer | Reaches |
|---|---|
| direct (e.g. 10 or 12 of them) | one data block each |
| **single indirect** | one block full of block pointers |
| **double indirect** | one block full of single-indirect pointers |
| **triple indirect** | one block full of double-indirect pointers |

- **On `open` the i-node is loaded into main memory**, and stays there while the file is open.

### Addressing

With a block size *B* and a pointer size *p*, one index block holds **k = B / p** pointers. The
maximum file size with *d* direct pointers and one pointer of each indirect level is

```
(d + k + k^2 + k^3) * B
```

The margin note in the notes gives the classic UNIX instance:
`(12 + 256 + 256² + 256³) × block size`, i.e. 12 direct pointers and k = 256.

## Worked example

Exercise S-3 (p31–p32): **10 direct pointers**, **one single-** and **one double-indirect**
pointer (each occupying one block), a **2 GiB** partition, **4 KiB** blocks, **8 B** per block
pointer.

**Pointers per index block:**

```
k = 4 KiB / 8 B = 4096 / 8 = 512 = 2^9
```

**a) Maximum file size:**

```
(10 + 512 + 512^2) * 4 KiB
 = 10 * 4 KiB  +  512 * 4 KiB  +  262144 * 4 KiB
 = 40 KiB      +  2 MiB        +  1 GiB
 = 1 GiB + 2 MiB + 40 KiB
```

**b) Blocks for the administration structures of a maximum-size file:**

```
1  i-node block
1  single-indirect block
1  first-level double-indirect block
512 second-level index blocks
---
515 blocks = 515 * 4 KiB = 2060 KiB
```

(The notes record **2056 KiB**, i.e. 514 × 4 KiB — the i-node block itself is evidently not counted
there. State the breakdown explicitly in the exam; the count of *index* blocks is 514, plus the
i-node itself.)

**c)** is a [[disk-block]] question, not an i-node one: 6 KiB files occupy two 4 KiB blocks each, so
2 GiB / 8 KiB = **2¹⁸ = 262 144** files fit. See [[disk-block]].

## Complexity / cost

Accesses to disk to reach logical block *i*, with the i-node already resident:

| Block range | Extra disk reads |
|---|---|
| first *d* | **0** |
| next *k* | 1 (the single-indirect block) |
| next *k*² | 2 |
| next *k*³ | 3 |

Hence the notes' verdict: **small files are fast; the speed sinks with increasing file size.** The
structure is deliberately biased towards the common case, since most files are small.

## Pros and cons

| | |
|---|---|
| (+) | only the i-nodes of **open** files are resident — the fix for FAT's central defect |
| (+) | **small files are fast**: no indirection at all |
| (+) | the index scales to enormous files without a large fixed cost |
| (+) | the whole data block is available for data |
| (−) | **speed sinks with file size** — up to three extra reads per access |
| (−) | administration overhead of its own: 515 blocks for a maximum-size file in the example above |

## Compared to

| | Index scope | Resident | Direct access | Growth |
|---|---|---|---|---|
| [[contiguous-allocation]] | start + length | — | **best** | **impossible** |
| [[linked-list-allocation]] | in the data blocks | — | **worst**, O(n) | free |
| [[file-allocation-table]] | whole medium | **always** | good | free |
| **i-node** | per file | **only when open** | good, degrading with size | free |

The relationship to memory management is exact: an i-node is to a
[[file-allocation-table|FAT]] what a [[multi-level-page-table]] is to a flat [[page-table]] — a
tree of index blocks that exists only where it is needed, trading extra indirections for a much
smaller resident footprint.

## Related

- [[directory]] — stores the i-node number, not the blocks.
- [[file-metadata]] — the attributes in the i-node.
- [[sparse-file]] — cheap in an i-node system, because an unused indirect pointer simply stays
  empty.
- [[multi-level-page-table]], [[page-table]], [[address-width-calculation]] — the calculation
  technique in a) and b) is exactly the one from chapter 04.

## Sources

- [[lecture-bs-5]] (p18, exercise S-3 on p31–p32)
