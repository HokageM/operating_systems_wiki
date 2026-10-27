---
title: Page Table Entry
type: concept
tags: [memory, paging]
chapter: "04"
sources: [lecture-bs-4]
created: 2026-09-30
updated: 2026-09-30
---

# Page Table Entry

## Definition

One row of a [[page-table]]: a **frame number plus a handful of status bits**, maintained partly
by the hardware and partly by the operating system. The lecture's 32-bit layout:

```
+---+---+---+------+---+--------------------------+
| C | R | M | Prot | P |   frame number           |
+---+---+---+------+---+--------------------------+
  1   1   1   1-3    1            20 bits
```

| Bit | Name | Meaning |
|---|---|---|
| **P** | Present/Absent | 1 = the page is in a frame; 0 = it is not → [[page-fault]] |
| **Prot** | Protection | access rights: Read / Write / Execute |
| **M** | Modified | the page has been written to → it is a [[dirty-page]] |
| **R** | Referenced | the page has been accessed (read or written) |
| **C** | Cacheable | whether the page may be held in the [[cache]] |

## Context

Belongs to [[page-table]]. The R and M bits are the raw material of the page-replacement
algorithms: [[not-recently-used]] classifies pages by (R, M), [[clock-page-replacement]] uses R
as its Use bit, and M decides whether an eviction needs a write-back.

## How it works

- **P** is checked by the [[memory-management-unit]] on every access; P = 0 raises an interrupt.
- **Prot** is checked in the same step; a violation also traps into the [[kernel]].
- **M** is set by the hardware on a write and cleared by the OS after the page has been written
  back to disk.
- **R** is set by the hardware on any access and cleared **periodically by the OS** — that
  periodic clearing is what makes R mean "used *recently*" rather than "used ever".
- **C** matters for memory-mapped device registers, where a cached stale value would be wrong.

### Sizing an entry (p28)

The split between frame number and status bits is not fixed — it follows from the machine:

```
frames  = 1 GiB / 8 KiB = 2^30 / 2^13 = 2^17   -> 17 bit for the frame number
entry   = 4 B = 32 bit
status  = 32 - 17 = 15 bit
```

## Related

- [[page-table]] — the array of these entries.
- [[dirty-page]] — the M bit made into a concept.
- [[not-recently-used]] / [[clock-page-replacement]] — the consumers of R and M.
- [[page-fault]] — what P = 0 causes.

## Common confusions

- **R and M are set by hardware, cleared by software.** Neither is maintained by the OS alone;
  the OS only *resets* them.
- **Both bits are reset for all of a process's pages at process start**, and R additionally at
  every periodic sweep.
- **The frame number field is not always 20 bits.** 20 is the lecture's example for a 32-bit
  machine with 4 KiB pages; the exercises derive it from the actual memory size.

## Sources

- [[lecture-bs-4]] (p10, p16, p22, p28)
