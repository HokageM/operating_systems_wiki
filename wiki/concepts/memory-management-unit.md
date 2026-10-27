---
title: Memory Management Unit (MMU)
type: concept
tags: [memory, hardware, addressing]
chapter: "04"
sources: [lecture-bs-4]
created: 2026-09-30
updated: 2026-09-30
---

# Memory Management Unit (MMU)

## Definition

The **Memory Management Unit (MMU)** is the hardware component that performs
[[address-translation]] — turning the virtual address the CPU emits into a physical address —
and that raises an [[interrupt]] when the access cannot be served.

## Context

Belongs to [[memory-management]]. It exists because translation must happen on *every* memory
access: a software implementation would multiply the cost of every instruction. It is the "(−)
expensive address translation → needs hardware support" that [[paging]] and [[virtual-memory]]
both list as their price.

## How it works

On each access the MMU:

1. Splits the virtual address into page number and offset.
2. Looks the page number up in the [[translation-lookaside-buffer]]; on a TLB miss it walks the
   [[page-table]] (possibly a [[multi-level-page-table]]) whose start address is held in a
   dedicated hardware register.
3. Checks the **Present/Absent bit** of the [[page-table-entry]]. If the page is not resident it
   raises an interrupt — a [[page-fault]] — and the OS takes over.
4. Checks the **protection bits** (Read / Write / Execute). A violation is likewise a trap into
   the OS.
5. Assembles the physical address and issues the access.

The register holding the page-table start address is part of the machine state and is **reset on
every process switch**, which is how each process gets its own address space — see
[[process-switch]].

## Related

- [[address-translation]] — what it computes.
- [[translation-lookaside-buffer]] — its internal cache.
- [[page-fault]] — the interrupt it generates.
- [[base-and-limit-register]] — the much simpler predecessor under partitioning.
- [[interrupt]] — the mechanism by which it hands control to the OS.

## Common confusions

- **The MMU does not load pages.** It only *detects* that a page is missing and interrupts; the
  **OS** performs the actual load from disk and updates the page table. Hardware detects,
  software resolves.
- **MMU vs. TLB.** The TLB is a component *inside* the MMU, not an alternative to it.

## Sources

- [[lecture-bs-4]] (p03, p05, p07, p09, p11)
