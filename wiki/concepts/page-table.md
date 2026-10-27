---
title: Page Table
type: concept
tags: [memory, paging]
chapter: "04"
sources: [lecture-bs-4, lecture-bs-5]
created: 2026-09-30
updated: 2026-09-30
---

# Page Table

## Definition

A **page table** (Seitentabelle) is a **list of frame numbers, one per process**. It has as many
entries as the process has pages, and its **i-th entry is the frame number of the i-th page**.
The page number is therefore not stored — it is the *index*.

## Context

Belongs to [[paging]]. It is the data structure [[address-translation]] consults, kept fast by
the [[translation-lookaside-buffer]] and kept small by the [[multi-level-page-table]] scheme.

## How it works

A dedicated **hardware register holds the start address of the current page table**, and this
register is **reset on every process switch** — that single register swap is what gives each
process its own address space. See [[process-switch]] and [[memory-management-unit]].

```
page number  --index-->  [ frame number | status bits ]  --> see page-table-entry
```

### Size

The table is indexed by page number, so it has one entry per page of the *virtual* address
space — for a 32-bit space with 4 KiB pages that is 2^20 entries **per process**. Page tables are
therefore very large. Two consequences the lecture draws:

1. Page tables themselves **lie in virtual memory**, so only the part currently needed has to be
   resident.
2. [[multi-level-page-table|Multi-level tables]] are used so that entries for unused regions need
   not exist at all.

### Worked example (p22)

8-bit virtual address, 3-bit page number, 6 pages, page/frame size 2^5 = 32 B:

| Page | Frame | P | M |
|---|---|---|---|
| 0 | 4 | 1 | 1 |
| 1 | 7 | 1 | 0 |
| 2 | 1 | 0 | 0 |
| 3 | 0 | 1 | 1 |
| 4 | 2 | 0 | 0 |
| 5 | 1 | 0 | 0 |

- Virtual address 53 = `001 10101` → page 1, P = 1 → frame 7 = `111` → physical `111 10101`.
  **No page fault.**
- Virtual address 84 = `010 10100` → page 2, **P = 0 → [[page-fault]]**. After the page has been
  loaded (here into frame 1 = `001`) the physical address is `001 10100`.

## Related

- [[page-table-entry]] — what one entry actually contains.
- [[multi-level-page-table]] — how the size problem is solved.
- [[translation-lookaside-buffer]] — the cache in front of it.
- [[address-translation]] — the procedure that uses it.
- [[file-allocation-table]] — the file-system analogue: one table for the whole medium, mapping logical unit numbers to physical ones.
- [[i-node]] — the per-file index, which is to a FAT what a [[multi-level-page-table]] is to a flat page table.

## Common confusions

- **One page table per process, not one per system.** Shared kernel mappings aside, the table is
  part of the process's state.
- **The entry is a frame number, not a physical address.** See [[address-translation]].
- **Entries for absent pages still exist.** A P = 0 entry is a valid entry; it simply says the
  page is not in a frame right now.

## Sources

- [[lecture-bs-4]] (p07, p08, p10, p22)
- [[lecture-bs-5]] (p18) — the FAT/i-node parallel.
