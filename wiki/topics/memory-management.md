---
title: Memory Management
type: topic
tags: [memory, allocation, paging]
chapter: "04"
sources: [lecture-bs-4]
created: 2026-09-30
updated: 2026-09-30
---

# Memory Management

## Summary

**Memory management** (Speicherverwaltung) is the OS's administration of main memory. The lecture
defines it by **four tasks**:

1. **Manage** free and occupied memory areas.
2. **Assign** areas to concurrently running processes at runtime.
3. **Release** areas after use or at process termination.
4. **Swap out** processes to the swap area on persistent storage (see [[swapping]]).

Underneath sits the [[memory-hierarchy]] — registers, [[cache|caches]], main memory, disk — and
above it sits the abstraction the OS sells to every process: a private, contiguous-looking
[[physical-and-virtual-memory|virtual address space]] that has nothing to do with where its data
physically lives.

## Why it exists

Several processes run concurrently and each believes it owns the machine. They must be
**separated** (one process must not read or corrupt another's memory), **accommodated** (more
memory may be demanded than exists), and **relocatable** (a process's position in memory may
change between and during runs — see [[relocation]]). None of this can be solved by the program
itself: a compiled program cannot know where it will be loaded. So the OS and the
[[memory-management-unit]] insert a translation layer between the addresses a program uses and
the addresses the memory chips see.

## Key concepts

- [[memory-hierarchy]] — the ordering of storage levels, and its cost/speed/capacity trade-off.
- [[memory-technologies]] — RAM, ROM, SRAM, DRAM: why the levels differ.
- [[cache]] and [[write-through-and-write-back]] — the hardware-managed level.
- [[relocation]] — a process may occupy different areas at different times.
- [[physical-and-virtual-memory]] — the central distinction of the chapter.
- [[address-translation]] and the [[memory-management-unit]] — how one becomes the other.
- [[base-and-limit-register]] — protection under partitioning.
- [[fragmentation]] — internal vs. external; the cost of every allocation scheme.
- [[page-and-frame]], [[page-table]], [[page-table-entry]], [[multi-level-page-table]] — the
  machinery of paging.
- [[page-size]] — the design parameter.
- [[address-width-calculation]] — the arithmetic every exercise demands.

## Mechanisms / algorithms

The chapter presents the allocation schemes as a numbered progression, each fixing the previous
one's defect:

| Variant | Scheme | Contiguous? | Fixes | Costs |
|---|---|---|---|---|
| **1** | [[static-partitioning]] | yes, fixed | — | internal [[fragmentation]], limited process count |
| **2** | [[dynamic-partitioning]] | yes, variable | exact allocation | external fragmentation, admin effort |
| **2a** | [[paging]] | **no** | external fragmentation | expensive translation → needs an [[memory-management-unit|MMU]] |
| **2b** | [[virtual-memory]] | no | the memory limit itself | page faults, replacement policy |

See [[virtual-memory]] for variant 2b, which is large enough to have its own topic page.

## Trade-offs

- **Simplicity vs. utilisation.** Static partitioning has a one-addition translation and wastes
  memory; paging uses memory almost perfectly and needs dedicated hardware on every access.
- **Internal vs. external fragmentation.** Every scheme trades one for the other; only paging
  reduces both, and only by introducing translation cost.
- **Contiguity vs. flexibility.** Dropping the requirement that a process occupy one connected
  block is the single decision that makes everything after it possible.
- **Hardware vs. software.** Translation, R/M bits and protection checks happen per access and
  must be hardware; loading pages, choosing victims and allocating frames happen per fault and
  can be software. The chapter draws this line repeatedly.

## Exam relevance

Very high — the chapter is arithmetic-heavy and the exercises are formulaic, which makes it the
most reliably scorable part of the module.

Question forms seen in the notes:

- **Definitions asked verbatim** (4-1): the memory hierarchy from the CPU's point of view;
  internal vs. external fragmentation; the advantages and the disadvantage of virtual memory; what
  a [[translation-lookaside-buffer|TLB]] is; the [[locality|Lokalitätsprinzip]].
- **Address arithmetic** (p06–p08, 4-5, Zusatz 1 & 2): given memory sizes and an address split,
  compute page size, page/frame counts, field widths, number of tables, status bits per entry. See
  [[address-width-calculation]].
- **Translate a concrete address** (4-4): binary in, binary out, plus "is this a page fault?".
- **Effective access time** (4-3, p13): a weighted sum over the hierarchy. See [[hit-rate]].
- **Compare replacement algorithms on one table** (4-6): LRU / NRU / Clock / optimal on the same
  four frames. See [[page-replacement]].
- **Explain a code fragment's locality** (4-2, p12): row-wise vs. column-wise array traversal.

## Open questions

- The lecture never names a *placement* strategy for [[dynamic-partitioning]] (first-fit,
  best-fit, worst-fit, buddy system). Either TUHH does not examine them or they appear in
  chapter 5.
- Segmentation (Segmentierung) is not mentioned anywhere in these notes, although it is standard
  in OS courses. Worth confirming it is out of scope.
- **Scheduling strategies** are still absent from the whole wiki after four chapters — see
  [[processes]].

## Sources

- [[lecture-bs-4]] — lecture part p01–p08, exercise recap 4-1 and Zusatz 1/2.
