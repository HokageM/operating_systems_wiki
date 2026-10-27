---
title: Memory Mapped File
type: concept
tags: [file-system, storage, virtual-memory]
chapter: "05"
sources: [lecture-bs-5]
created: 2026-09-30
updated: 2026-09-30
---

# Memory Mapped File

## Definition

A [[file]] that is **mapped into the virtual address space** of a process, so that it can be read
and written **without `read()` and `write()`** — ordinary memory accesses to the mapped region
operate on the file.

## Context

The meeting point of [[file-systems]] and [[virtual-memory]]. The file's blocks become the backing
store of a range of pages, exactly as the swap area is the backing store of anonymous pages. A
[[page-fault]] on a mapped page loads a block from the file; evicting a
[[dirty-page|modified page]] writes it back.

## Pros and cons

| | |
|---|---|
| (+) | **faster** |
| (+) | **no access to the external medium** for data already resident, and **no system calls** |
| (+) | **simple access via pointers** |
| (+) | **several processes can change the file directly in main memory** — shared memory for free |
| (−) | **page faults can cause delay** |

The "no system calls" advantage is the substantial one: every `read()` is a [[trap]] into the
[[kernel]] plus a copy from a kernel buffer into the process's buffer. A mapped access is neither —
the [[memory-management-unit]] does the work in hardware, and the data is never copied.

The disadvantage is the same property seen from the other side: the cost is no longer visible in
the source code. An innocuous pointer dereference may take milliseconds — the same loss of
predictability that [[virtual-memory]] introduces generally.

## Related

- [[virtual-memory]] — the mechanism it rides on.
- [[page-fault]] — what a first access to a mapped page raises.
- [[dirty-page]] — how a write eventually reaches the disk.
- [[page-locking]] — shared-memory mappings are one of the two cases in which frames are excluded
  from replacement.
- [[interprocess-communication]] — a shared mapping is a shared-memory IPC channel.
- [[race-condition]] and [[mutual-exclusion]] — several processes writing the same mapping need
  the full synchronisation apparatus of chapter 03; the mapping itself provides none.
- [[asynchronous-file-access]] — the other way to avoid blocking on I/O.

## Common confusions

- **Mapping a file does not load it.** The mapping is established by manipulating the
  [[page-table]]; blocks arrive one [[page-fault]] at a time ([[demand-paging]]).
- **A write to a mapping is not immediately on disk.** It reaches the disk when the page is written
  back — which is why a [[journaling-file-system]] and explicit flushing still matter.

## Sources

- [[lecture-bs-5]] (p13)
