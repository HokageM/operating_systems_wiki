---
title: Process Image
type: concept
tags: [process, memory, data-structures]
chapter: "01"
sources: [lecture-bs-1, lecture-bs-4]
created: 2026-09-30
updated: 2026-09-30
---

# Process Image

## Definition

The **process image** (Prozessimage) is the **total representation of a [[process]] in memory**.

**Contents:** the [[process-control-block|PCB]], user data (Benutzerdaten), the program, and the
stacks.

## Context

Belongs to [[processes]]. It is what the OS management structures point at, and what [[swapping]]
moves out to persistent storage.

## How it works

The OS keeps a **primary process table** with one entry per process (P1, P2, ...); each entry
refers to that process's image. Alongside it are a **memory table** (Speichertabelle) and an **I/O
table**.

```
Processes → Primary process table → [P1] → Process image of P1
                                    [P2] → Process image of P2
```

The memory areas a process occupies are: **program memory** (Programmspeicher), memory for
**global and static local variables**, the **heap**, and the **stack**.

Under [[paging]] the image is **not contiguous in physical memory**: it is cut into
[[page-and-frame|pages]] scattered over frames, and under [[virtual-memory]] only some of those
pages are resident at all. The image remains contiguous in the process's *virtual* address space,
which is exactly the abstraction [[address-translation]] maintains.

## Related

- [[process-control-block]] — one component of the image.
- [[swapping]] — moves the image out of main memory.
- [[fork]] — creates a copy of the image (or, with [[copy-on-write]], defers the copy).
- [[exec]] — replaces program, data, heap and stack, but keeps the process identity.
- [[paging]] — how the image is laid out in physical memory.
- [[memory-management]] — the four tasks that administer it.

## Common confusions

- **Image vs. PCB** — the PCB is *inside* the image. Naming only the PCB when asked for the image
  loses the user data / program / stacks marks.
- The heap and the stack grow at run time; the image is therefore not of fixed size.
- **Contiguous in virtual memory, scattered in physical memory.** The image looks like one block
  only from inside the process; see [[physical-and-virtual-memory]].

## Sources

- [[lecture-bs-1]] (p14, p15)
