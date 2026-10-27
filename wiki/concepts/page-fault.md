---
title: Page Fault
type: concept
tags: [memory, paging, virtual-memory]
chapter: "04"
sources: [lecture-bs-4, lecture-bs-5]
created: 2026-09-30
updated: 2026-09-30
---

# Page Fault

## Definition

A **page fault** (Seitenfehler) occurs when a process accesses a page that **is not located in any
frame** — i.e. the Present/Absent bit of its [[page-table-entry]] is 0. The
[[memory-management-unit]] detects this and **raises an [[interrupt]]**; the operating system
then loads the page from persistent storage into a free frame and updates the page table.

## Context

Belongs to [[virtual-memory]]. It is the event that makes virtual memory work — and the event
whose frequency decides whether it works *well*. See [[thrashing]] and
[[page-fault-frequency]].

## How it works

1. The process issues a virtual address.
2. The MMU finds P = 0 in the page-table entry → **interrupt**.
3. The OS locates the page in the swap area on persistent storage.
4. It finds a **free frame**. If none is free, a [[page-replacement]] algorithm selects a victim
   frame and frees it; if the victim is a [[dirty-page]], it must be **written back first**.
5. The page is read into the frame, the page-table entry is updated (frame number, P = 1), and a
   [[translation-lookaside-buffer|TLB]] entry is written.
6. The **last instruction is repeated** — the process resumes exactly where it was interrupted
   and this time the access succeeds.

Step 6 is what makes the whole mechanism transparent: the program never learns that it faulted.

## Cost

Loading from disk costs milliseconds against nanoseconds for a memory access — a page fault is
roughly five orders of magnitude more expensive than a hit. That asymmetry dominates the
arithmetic in every effective-access-time exercise (see [[hit-rate]]).

## Related

- [[demand-paging]] — the policy of loading *only* on a page fault.
- [[page-replacement]] — how the victim frame is chosen when memory is full.
- [[dirty-page]] — why some evictions cost twice.
- [[thrashing]] — what happens when faults become the normal case.
- [[interrupt]] — the delivery mechanism.
- [[memory-mapped-file]] — a page fault on a mapped page loads a block from a [[file]] instead of from the swap area.

## Common confusions

- **A page fault is not an error.** Despite the name (and despite being delivered as an
  interrupt), it is the *normal* operation of [[demand-paging]]. An access to an address the
  process is not allowed to touch is a **protection** violation, which is a different thing.
- **The MMU does not fix it.** Hardware detects, the OS resolves. See [[memory-management-unit]].
- **Page fault vs. TLB miss.** A TLB miss is resolved by reading the page table — a few hundred
  nanoseconds. A page fault requires disk I/O — milliseconds. Every page fault involves a TLB
  miss, but almost no TLB miss is a page fault.

## Sources

- [[lecture-bs-4]] (p09, p11, p12, p22)
- [[lecture-bs-5]] (p13) — page faults as the one disadvantage of memory-mapped files.
