---
title: Page and Frame
type: concept
tags: [memory, paging]
chapter: "04"
sources: [lecture-bs-4]
created: 2026-09-30
updated: 2026-09-30
---

# Page and Frame

## Definition

The two units of [[paging]], of **identical size**:

- A **page** (Seite) is a block of a process's **virtual** address space.
- A **frame** (Seitenrahmen, often just Rahmen) is a block of **physical** main memory.

A page is *placed into* a frame. The number of pages and the number of frames are generally
different — usually there are far more pages than frames, which is what makes [[virtual-memory]]
interesting.

## Context

Belongs to [[paging]]. The page/frame distinction is the memory-side instance of the general
[[physical-and-virtual-memory]] split: page = virtual, frame = physical.

## How it works

With a page size of `2^m` bytes:

```
number of pages  = size of the virtual address space  / page size
number of frames = size of the physical main memory   / frame size
```

The **frame number** needs `log2(number of frames)` bits; the **page number** needs
`log2(number of pages)` bits. See [[address-width-calculation]].

### Worked example (p06)

1 GiB physical memory, 64 KiB frames, 4 GiB virtual address space:

```
frames = 2^30 / 2^16 = 2^14 = 16384
pages  = 2^32 / 2^16 = 2^16 = 65536
```

### Worked example (p22)

8-bit virtual address with a 3-bit page number ⇒ offset = 8 − 3 = 5 bit ⇒
page size = frame size = `2^5` = 32 B.

## Related

- [[paging]] — the scheme.
- [[page-table]] — maps page numbers to frame numbers.
- [[page-size]] — the trade-off in choosing `2^m`.
- [[address-translation]] — why equal sizes let the offset pass through unchanged.

## Common confusions

- **Page vs. frame is a constant exam trap.** "Wie viele Rahmen?" asks about *physical* memory,
  "wie viele Seiten?" about the *virtual* address space. The page size is the same number in both
  divisions, but the dividend is not.
- **A page number is not a frame number.** The page table converts one into the other; they are
  usually different widths, because virtual and physical memory have different sizes.

## Sources

- [[lecture-bs-4]] (p05, p06, p22, p23, p28)
