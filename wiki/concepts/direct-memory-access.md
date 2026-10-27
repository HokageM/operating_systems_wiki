---
title: Direct Memory Access (DMA)
type: concept
tags: [io, dma, hardware]
chapter: "01"
sources: [lecture-bs-1, lecture-bs-4, lecture-bs-5]
created: 2026-09-30
updated: 2026-09-30
---

# Direct Memory Access (DMA)

## Definition

**Direct Memory Access** is the I/O technique in which the **data exchange happens directly
between the I/O device and main memory**: the processor grants the I/O module access to memory and
is then no longer needed for every word.

## Context

The third and most efficient of the three I/O techniques in [[interrupts-and-io]], after
programmed I/O and interrupt-driven I/O.

## How it works

1. The CPU issues a "read block" command to the I/O module, passing it:
   - the **type of operation**,
   - the **address of the I/O device**,
   - the **number of words** to be transferred.
2. The CPU is free for other activities for the whole operation.
3. The DMA module copies a **whole block** directly between the external device and memory.
4. When the action ends, the DMA module raises an [[interrupt]].
5. The CPU reads the status of the DMA module.

```
CPU → DMA : "read block"  ⇒ CPU does something else
DMA → CPU : interrupt     ⇒ CPU reads DMA status
```

## Related

- [[interrupt]] — how completion is signalled.
- [[device-driver]] — the code that programs the DMA controller.
- [[locality]] — block transfer only pays off because spatial locality holds.
- [[page-locking]] — a frame targeted by a running DMA transfer must be excluded from
  [[page-replacement]]; the DMA controller holds a **physical** address and knows nothing about
  page tables.
- [[asynchronous-file-access]] — what makes a background transfer cost the CPU nothing.
- [[raid]] — the controller issues its decomposed physical requests this way.

## Common confusions

- **DMA does not eliminate the interrupt** — it eliminates the *per-word* CPU involvement; there
  is still one interrupt per block.
- **Interrupt-driven I/O is not DMA.** With interrupt-driven I/O every word still passes through
  the processor; that is exactly its stated drawback.
- DMA is not free: it needs dedicated hardware and competes with the CPU for the memory bus.
- **DMA bypasses address translation.** It operates on physical addresses, which is why it
  interacts with [[virtual-memory]] only through [[page-locking]].

## Sources

- [[lecture-bs-1]] (p06)
- [[lecture-bs-4]] (p17) — DMA as a reason for page locking
- [[lecture-bs-5]] (p13, p20) — background transfers for asynchronous file access and RAID.
