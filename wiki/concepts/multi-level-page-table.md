---
title: Multi-Level Page Table
type: concept
tags: [memory, paging]
chapter: "04"
sources: [lecture-bs-4, lecture-bs-5]
created: 2026-09-30
updated: 2026-09-30
---

# Multi-Level Page Table

## Definition

A **multi-level page table** splits the page number into several fields, each indexing a table at
one level. The first-level table is always resident; second-level tables exist **only for those
regions of the address space the process actually uses**. This removes the need to materialise one
enormous flat [[page-table]] per process.

## Context

Belongs to [[page-table]]. It is the answer to the size problem: a flat table for a 32-bit
address space with 4 KiB pages needs 2^20 entries per process, most of which describe address
space that is never touched.

## How it works

Two-level layout for a 32-bit address with 4 KiB pages:

```
+-----------+-----------+-------------------+
|  PT1 (10) |  PT2 (10) |   offset (12)     |
+-----------+-----------+-------------------+
```

1. **PT1** indexes the **first-level table**.
2. The value found there is a **frame number** (not a physical address); padded with the offset
   width of zeros it becomes the start address of a **second-level table**.
3. **PT2** indexes that second-level table, yielding the frame number of the target page.
4. The **offset** is appended unchanged.

The key economy: not all first-level entries need to point anywhere. A process using 23 MiB out
of a 4 GiB address space materialises only a handful of second-level tables.

### Worked example (p23)

32-bit system, 9 bits for level 1, 10 bits for level 2, virtual space 4 GiB, physical 1 GiB:

```
offset      = 32 - 9 - 10 = 13 bit  ->  page size = 2^13 = 8 KiB
frames      = 1 GiB / 8 KiB = 2^17  ->  frame number is 17 bit wide
entry       = P | M | rights | frame number
```

For a process of **23 MiB**:

```
pages needed = 23 MiB / 8 KiB = 23 * 2^7 = 2944 pages
entries per second-level table = 2^10 = 1024
second-level tables = ceil(2944 / 1024) = 3
total = 3 second-level + 1 first-level = 4 tables
```

### Worked example (p27)

32-bit addresses, 8 bits level 1, 11 bits level 2, main memory 1 GiB, entry size 4 B:

```
a) page size          = 2^(32-8-11) = 2^13 B = 8 KiB
b) pages per process  = 2^32 / 2^13 = 2^19 = 524 288
c) maximum table size = 1 first-level table + 2^8 = 256 second-level tables
                      = 257 tables * 8 KiB = 2056 KiB
```

Part (c) uses the stated assumption that **each table occupies exactly one page**.

### Cost

Every level adds one main-memory access to an untranslated lookup. For a two-level table
(p28): 1 (first-level table) + 1 (second-level table) + 1 (data) = **3 main-memory accesses** to
read one byte — which is precisely why the [[translation-lookaside-buffer]] exists.

## Related

- [[page-table]] — the flat version.
- [[translation-lookaside-buffer]] — what hides the extra lookups.
- [[address-width-calculation]] — the arithmetic above.
- [[i-node]] — the same structure in a file system: a tree of index blocks, so that only the populated parts exist and only open files' indexes are resident.
- [[sparse-file]] — the file-system version of an unmapped region.

## Common confusions

- **The bits split the *page number*, not the whole address.** The offset is fixed by the page
  size; PT1 + PT2 + offset = address width, so the offset is what is *left over*.
- **Entries hold frame numbers.** They must be shifted/padded to become addresses.
- **"How many tables" ≠ "how many entries".** Exercises ask both; divide by the entries per table
  and round **up**, then remember to add the single first-level table.

## Sources

- [[lecture-bs-4]] (p10, p23, p27, p28)
- [[lecture-bs-5]] (p18, exercise S-3) — i-nodes as a multi-level index; the same sizing arithmetic.
