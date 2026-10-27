---
title: Page Locking
type: concept
tags: [memory, virtual-memory, paging]
chapter: "04"
sources: [lecture-bs-4, lecture-bs-5]
created: 2026-09-30
updated: 2026-09-30
---

# Page Locking

## Definition

**Page locking** (Seitensperrung) means that individual frames can be **excluded from the
replacement procedure** — they may not be chosen as a victim by any [[page-replacement]]
algorithm, no matter how long they have gone unused.

## Context

Belongs to [[virtual-memory]]. It is a correctness constraint sitting on top of the replacement
policy: some frames simply must not move.

## How it works

The lecture gives two reasons:

1. **DMA transfers.** A [[direct-memory-access]] controller writes into a *physical* address it
   was given when the transfer was started. It knows nothing about page tables and cannot be
   interrupted by a [[page-fault]]. If the OS evicted that frame mid-transfer, the device would
   overwrite whatever page landed there. The frame must stay put until the transfer completes.
2. **Shared pages / shared memory.** A frame mapped into several processes' address spaces is
   reachable through several page tables. Evicting it on behalf of one process affects all of
   them, so such frames are handled specially.

Kernel code and data are locked for the same reason: the page-fault handler itself cannot be
allowed to fault.

## Related

- [[page-replacement]] — the procedure locking overrides.
- [[direct-memory-access]] — reason 1.
- [[interprocess-communication]] — shared memory is reason 2.
- [[copy-on-write]] — another case where one frame is reachable from several page tables.
- [[asynchronous-file-access]] — the buffer of an in-flight transfer is exactly one of the cases that must be locked.
- [[memory-mapped-file]] — shared mappings are the other.

## Common confusions

- **Page locking is not a [[lock]].** It has nothing to do with mutual exclusion between
  processes; it locks a page *into memory*, not *against access*.
- **Locked frames reduce the pool.** Too many locked frames shrink the memory available for
  everything else and can bring on [[thrashing]].

## Sources

- [[lecture-bs-4]] (p17)
- [[lecture-bs-5]] (p13) — asynchronous I/O and shared mappings as the two locking cases.
