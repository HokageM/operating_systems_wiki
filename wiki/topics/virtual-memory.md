---
title: Virtual Memory
type: topic
tags: [memory, virtual-memory, paging]
chapter: "04"
sources: [lecture-bs-4, lecture-bs-5]
created: 2026-09-30
updated: 2026-09-30
---

# Virtual Memory

## Summary

**Virtual memory** (virtueller Speicher) — *variant 2b* of memory allocation — is
[[paging]] **plus the Present/Absent bit**. Not all pages of a process need to be in main memory;
the rest lie on persistent storage and are called **swapped-out pages**. Any access to a
non-resident page raises a [[page-fault]], the OS loads the page into a free frame, and the
process resumes as if nothing had happened. Because the page-to-frame assignment can change over
time, a process's memory is no longer tied to any particular physical location at all.

This single mechanism gives the module its most powerful abstraction: **every process behaves as
though it had the whole address space to itself**, regardless of how much memory the machine
actually has.

## Why it exists

Under plain [[paging]] a process still has to fit entirely into main memory. Virtual memory
removes that last constraint. The notes list three advantages and one disadvantage:

| | |
|---|---|
| (+) | **more memory available than physically present** |
| (+) | **physical addresses may be larger than virtual ones** |
| (+) | **many processes can hold frames in main memory simultaneously** |
| (−) | **slow — requires hardware support** |

The third advantage is the one that matters for throughput: because no process needs all of its
pages, many processes can be resident at once, and the CPU always has someone to run.

## Key concepts

- [[page-fault]] — the event the whole technique is built around.
- [[demand-paging]] and [[prepaging]] — the two **fetch** policies.
- [[page-replacement]] — the **replacement** policy.
- [[dirty-page]] — why some evictions cost twice.
- [[translation-lookaside-buffer]] — what makes translation affordable.
- [[hit-rate]] — how the whole arrangement is measured.
- [[working-set]] and [[thrashing]] — when it works and when it collapses.
- [[page-locking]], [[local-and-global-page-replacement]], [[page-fault-frequency]] — the three
  refinements on p17.
- [[locality]] — the property everything above depends on.

## Mechanisms / algorithms

### The three policy questions

| Question | Name | Answers in this chapter |
|---|---|---|
| When to load a page? | **fetch policy** | [[demand-paging]], [[prepaging]] |
| Which page to evict? | **replacement policy** | [[lru-page-replacement]], [[clock-page-replacement]], [[not-recently-used]], [[optimal-page-replacement]] |
| From whose pages, and how many frames each? | **allocation policy** | [[local-and-global-page-replacement]], [[page-fault-frequency]], [[page-locking]] |

### The replacement algorithms

All of them are attempts to approximate [[optimal-page-replacement|the optimum]], which needs the
future, ever more cheaply:

```
optimal  (impossible)
   |  approximate the future by the recent past
   v
LRU      (exact recency, expensive, needs hardware)
   |  approximate recency by one bit
   v
Clock    (one Use bit + a rotating hand)
   |  add the write-back cost
   v
NRU      (R and M bits -> four classes, pick at random from the lowest)
```

## Trade-offs

- **Transparency vs. predictability.** The process never learns that it faulted — which is
  wonderful for the programmer and terrible for real-time guarantees, since any instruction may
  take milliseconds.
- **Over-commitment vs. thrashing.** Admitting more processes raises utilisation right up to the
  point where no [[working-set]] fits any more, at which it collapses. See [[thrashing]].
- **Accuracy vs. cost of the replacement decision.** LRU knows the most and costs the most; NRU
  knows two bits and is nearly free. The lecture's verdict on NRU — "not optimal but acceptable" —
  is the honest summary of the whole family.
- **Local vs. global allocation.** Isolation against adaptability; see
  [[local-and-global-page-replacement]].

## Exam relevance

High. The recurring forms:

- **Definitions** (4-1 d, 4-1 c): what a TLB is; the advantages *and* the disadvantage of virtual
  memory. The notes give exactly three advantages and one disadvantage — a list worth memorising
  as a list.
- **Address translation with a page-fault check** (4-4): translate two addresses, one of which has
  P = 0. See [[page-table]].
- **Multi-level table sizing** (4-5, Zusatz 2): page size, page count, number of tables, status
  bits per entry, and the number of main-memory accesses per byte read. See
  [[multi-level-page-table]].
- **Effective access time** (4-3, p13). See [[hit-rate]].
- **Run four replacement algorithms on one table** (4-6). Best single revision exercise in the
  chapter, because the four answers differ — see [[optimal-page-replacement]].
- **Code locality** (4-2): explain why `for(i) for(j) a[i][j]` beats `for(j) for(i) a[i][j]`. See
  [[locality]].

## Open questions

- The page-fault handling steps are described but never numbered the way the 8-step
  [[process-switch]] is. If the exam asks for an enumerated procedure, the order in
  [[page-fault]] is the one reconstructed from p09 and p11.
- FIFO and second-chance are not named in the notes, although the "loaded at" column of exercise
  4-6 exists only to support a FIFO answer. Worth asking whether FIFO is examinable.

## Connections to chapter 05

The file-system chapter reuses this chapter's machinery almost point for point:

| Chapter 04 | Chapter 05 |
|---|---|
| [[page-and-frame]] | [[disk-block]] |
| [[page-size]] trade-off | block-size trade-off |
| [[page-table]] | [[file-allocation-table]] |
| [[multi-level-page-table]] | [[i-node]] |
| unmapped page range | [[sparse-file]] |
| [[dynamic-partitioning]] failing | [[contiguous-allocation]] failing |
| [[swapping]] to the swap area | [[memory-mapped-file]], paging to a real file |

The [[memory-mapped-file]] is the point where the two chapters actually meet rather than merely
rhyme: it maps a [[file]] into the virtual address space, so that [[demand-paging]] and
[[page-fault]] handling do the file I/O and no 
ead()/write() calls are needed at all.

## Sources

- [[lecture-bs-4]] — lecture part p08–p17, exercise recap 4-1 to 4-6, Zusatz 2.
- [[lecture-bs-5]] (p13) — memory-mapped files; the file-system parallels above.
