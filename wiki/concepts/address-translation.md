---
title: Address Translation
type: concept
tags: [memory, addressing, paging]
chapter: "04"
sources: [lecture-bs-4]
created: 2026-09-30
updated: 2026-09-30
---

# Address Translation

## Definition

**Address translation** is the conversion of a **virtual address** issued by a program into the
**physical address** of the memory cell actually accessed. It happens on *every* memory access,
which is why it is implemented in hardware — the [[memory-management-unit]].

## Context

Belongs to [[memory-management]]. It is the mechanism that makes [[relocation]] and the
[[physical-and-virtual-memory]] split possible. Under [[paging]] it is table-driven; under
[[static-partitioning]] it is a single addition.

## How it works

### Under partitioning

```
physical address = base register + virtual address
```

A single addition, checked against the limit register — see [[base-and-limit-register]]. This is
the "(+) simple address translation" advantage of [[static-partitioning]].

### Under paging

Let the page size be `2^m` bytes and virtual addresses be `n` bits wide. The address splits
without any arithmetic, purely by cutting the bit string:

```
 n bits total
+-------------------------+-------------------+
|   page number (n-m)     |   offset (m)      |
+-------------------------+-------------------+
```

1. The **page number** is the index into the [[page-table]].
2. The entry read there is the **frame number**.
3. The **offset** is carried over unchanged — the position within a page equals the position
   within a frame, because page size = frame size.

```
physical address = (frame number << m) | offset
                 = frame start address + offset
```

### Worked example (p08)

Virtual address space 2^16 B, physical memory 2^12 B, frames of 2^8 B.
→ offset = 8 bit, page number = 16 − 8 = 8 bit, frame number = 12 − 8 = 4 bit.

Page table:

| Page | Frame |
|---|---|
| 0 | 0100 |
| 1 | 0101 |
| 2 | 0010 |
| 3 | 0000 |
| 4 | 1111 |
| 5 | 1001 |

Virtual address `0000 0100 | 1110 1001`:

```
page number = 0000 0100 = 4      ->  frame = 1111
offset      = 1110 1001          ->  unchanged
physical    = 1111 1110 1001
```

Note that the physical address is 12 bits while the virtual one was 16 — physical memory is
smaller here, which is exactly the normal case.

## Related

- [[memory-management-unit]] — the hardware.
- [[page-table]] / [[multi-level-page-table]] — the data structure consulted.
- [[translation-lookaside-buffer]] — the cache that makes the common case fast.
- [[address-width-calculation]] — how the bit widths themselves are derived.

## Common confusions

- **The offset is never translated.** Only the page number is looked up; the low bits pass
  through untouched. Students who "translate the whole address" lose the example.
- **The page-table entry is a frame *number*, not a physical address.** It must be shifted left by
  `m` bits (equivalently: padded with `m` zeros) to become the frame's start address.
- **Number of bits ≠ number of pages.** `n − m` bits of page number means `2^(n−m)` pages.

## Sources

- [[lecture-bs-4]] (p06, p07, p08, p10, p22)
