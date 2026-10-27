---
title: Dirty Page
type: concept
tags: [memory, paging, virtual-memory]
chapter: "04"
sources: [lecture-bs-4]
created: 2026-09-30
updated: 2026-09-30
---

# Dirty Page

## Definition

A **dirty page** is a page that has been **modified** since it was loaded into its frame — the
**M bit** (Modified) of its [[page-table-entry]] is set. Its copy in main memory and its copy on
persistent storage differ.

## Context

Belongs to [[virtual-memory]]. It is the page-level instance of the
[[write-through-and-write-back|write-back]] policy already seen one level up in the [[cache]].

## How it works

The M bit is **set by the hardware** on any write access and **cleared by the OS** once the page
has been written back.

It matters at exactly one moment: **eviction**.

- **M = 0** — the frame's contents are identical to the disk copy. The frame can simply be
  overwritten; eviction is free.
- **M = 1** — the page **must be written back to persistent storage first**. Eviction costs a
  full disk write *before* the new page can even be read in, i.e. two disk operations instead of
  one.

This makes M a first-class input to [[page-replacement]]: [[not-recently-used]] explicitly
prefers a clean page over a dirty one of otherwise equal standing, and
[[clock-page-replacement]] in its extended forms does the same.

There is one further bookkeeping detail (p11): if a [[translation-lookaside-buffer|TLB]] entry
whose modified bit is set is replaced, that modified bit must be **propagated into the page table
in main memory** — otherwise the write-back would be skipped and data lost.

## Related

- [[page-table-entry]] — where the M bit lives.
- [[write-through-and-write-back]] — the same idea at the cache level.
- [[page-replacement]] — dirty pages are the expensive victims.
- [[page-locking]] — another reason a frame may be a bad eviction candidate.

## Common confusions

- **Dirty ≠ recently used.** M says *written*, R says *accessed*. A page can be dirty and
  long-unused (a prime eviction candidate that is nevertheless expensive to evict).
- **Reading a page does not make it dirty.** Only writes set M; reads set R.

## Sources

- [[lecture-bs-4]] (p09, p11, p16)
