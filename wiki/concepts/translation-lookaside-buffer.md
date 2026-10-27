---
title: Translation Lookaside Buffer (TLB)
type: concept
tags: [memory, paging, caching, hardware]
chapter: "04"
sources: [lecture-bs-4]
created: 2026-09-30
updated: 2026-09-30
---

# Translation Lookaside Buffer (TLB)

## Definition

The **Translation Lookaside Buffer (TLB)** is a **cache holding the assignment of page numbers to
frame numbers**. One TLB entry corresponds to one [[page-table-entry]] — one entry per virtual
page. It is **fully associative**: all entries are queried **in parallel**, so a lookup takes
constant time regardless of where the entry sits.

## Context

Belongs to [[virtual-memory]], inside the [[memory-management-unit]]. Without it every memory
access would cost one extra memory access per page-table level — three accesses to read one byte
under a two-level [[multi-level-page-table]] (p28). The TLB turns the common case back into one.

## How it works

```
virtual address
      |
      v
  [ TLB lookup ]  --hit-->  frame number | offset  -->  memory access
      |
    miss
      |
      v
  [ page table ]  --P=1-->  frame number, replace a TLB entry, retry
      |
     P=0
      |
      v
  [ page fault ] --> OS loads the page --> repeat the last instruction
```

- **Hit:** the frame number is concatenated with the offset and the access proceeds.
- **Miss:** the [[page-table]] is consulted; the entry found there **replaces** a TLB entry.
- **Page fault:** an [[interrupt]] is generated, the page is loaded, and the **last instruction is
  repeated**.

### Replacement

The entry **unused the longest** is replaced — i.e. [[lru-page-replacement|LRU]] applied to the
TLB. Important bookkeeping detail: if the victim entry's **modified bit is set**, that bit must be
set in the [[page-table]] in main memory before the entry is discarded. Otherwise the
[[dirty-page]] would later be evicted without being written back.

### Lookup costs (p12)

| Where the page is found | Cost |
|---|---|
| in the TLB | 1 TLB access + 1 memory access |
| in main memory (TLB miss) | 1 TLB + 1 page-table + 1 memory access + TLB update |
| on disk (page fault) | 1 TLB + 1 page-table + 1 memory + **1 disk access** + TLB update |

## Related

- [[cache]] — the TLB is a cache, of translations rather than data.
- [[hit-rate]] — how its effectiveness is measured.
- [[page-table]] — what it caches.
- [[locality]] — a high TLB hit rate requires good locality.
- [[process-switch]] — each process has its own translations, so a switch invalidates the TLB.

## Common confusions

- **TLB vs. cache.** The TLB caches **address translations**; the [[cache]] caches **data**. An
  access can hit one and miss the other.
- **TLB miss vs. page fault.** A miss costs one extra memory access; a fault costs a disk access.
  See [[page-fault]].
- **"Fully associative" is not a detail.** It is why the TLB is small: comparing every entry in
  parallel is expensive hardware, so TLBs hold only tens to hundreds of entries.

## Sources

- [[lecture-bs-4]] (p11, p12, p19, p28)
