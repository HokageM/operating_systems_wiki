---
title: Physical and Virtual Memory
type: concept
tags: [memory, addressing]
chapter: "04"
sources: [lecture-bs-4]
created: 2026-09-30
updated: 2026-09-30
---

# Physical and Virtual Memory

## Definition

- **Physical memory** (physikalischer Speicher) is the memory **actually built into** the machine.
- **Virtual memory** (virtueller Speicher) is the memory the **operating system makes available**
  to a process.

Correspondingly:

- A **physical address** (also *absolute address*) designates a location in the memory that really
  exists.
- A **virtual address** (also *logical address*) is the address a program uses. It must be
  translated into a physical address before the access can be carried out.

## Context

Belongs to [[memory-management]] and is the core distinction of the whole chapter.
[[relocation]] makes it necessary; [[address-translation]] and the [[memory-management-unit]]
implement it; [[virtual-memory]] as a *technique* (variant 2b) exploits it by letting the virtual
space be larger than the physical one.

## How it works

Every program is written and compiled entirely in virtual addresses. On every access the hardware
translates:

```
virtual address  --[ MMU + page table ]-->  physical address
```

Because this happens on *every* memory access, it must be very efficient — hence the dedicated
[[memory-management-unit]] and the [[translation-lookaside-buffer]].

The two spaces are independent in size:

- virtual **larger** than physical — the normal case; the excess lives on disk (see
  [[virtual-memory]]).
- physical **larger** than virtual — also possible and explicitly listed as an advantage in the
  notes: many processes can hold frames in main memory simultaneously even though each individual
  process can only address a smaller space.

### Worked example (p06)

1 GiB physical memory, 64 KiB frames, 4 GiB virtual address space:

```
frames = 1 GiB / 64 KiB = 2^30 / 2^16 = 2^14 = 16384
pages  = 4 GiB / 64 KiB = 2^32 / 2^16 = 2^16 = 65536
```

## Related

- [[address-translation]] — how one becomes the other.
- [[memory-management-unit]] — the hardware that does it.
- [[virtual-memory]] — the technique of not keeping all pages resident.
- [[address-width-calculation]] — the arithmetic used in every exercise of this chapter.

## Common confusions

- **"Virtual memory" means two things.** (1) The *address space* the OS offers, as defined here;
  (2) the *technique* of holding only part of it in main memory and the rest on disk
  ([[virtual-memory]], variant 2b). Paging alone already uses virtual addresses without being
  "virtual memory" in the second sense.
- **Virtual is not automatically bigger.** The notes explicitly list "physical addresses can be
  larger than virtual ones" as a possibility.

## Sources

- [[lecture-bs-4]] (p03, p06, p08, p18)
