---
title: Sparse File
type: concept
tags: [file-system, storage]
chapter: "05"
sources: [lecture-bs-5]
created: 2026-09-30
updated: 2026-09-30
---

# Sparse File

## Definition

A **sparse file** (dünn besetzte Datei) is a file with **regions of undefined content or null
bytes**, which is **stored compactly**: **only the blocks that actually contain data are created**.

## How it works

Writing at offset 1 GiB in an empty file does not allocate a gigabyte. The allocation structure
simply has no entry for the intervening logical blocks; a read of them returns zero bytes. In an
[[i-node]] system this is natural — an unused pointer is just empty — and the file's reported size
(from its [[file-metadata]]) can be far larger than the storage it occupies.

## Context

Belongs to [[file-systems]]. It is the file-system analogue of an unmapped region of a virtual
address space: a logical range that exists in the addressing scheme but has no physical storage
behind it, and which materialises only on first write.

## Related

- [[i-node]] — the allocation method that supports sparse files most naturally.
- [[file-metadata]] — the reported size, which is now decoupled from the space used.
- [[virtual-memory]] and [[page-table]] — the same idea: a Present bit of 0 means "no physical
  storage for this logical unit". [[multi-level-page-table]] is the direct structural parallel,
  where second-level tables exist only where needed.
- [[copy-on-write]] — the same "allocate only when actually written" principle.
- [[disk-block]] — the unit that is or is not created.

## Common confusions

- **A sparse file is not compressed.** Nothing is encoded; the empty blocks simply do not exist.
  Copying a sparse file with a naive tool expands it to its full size, because the reads return
  real zero bytes.

## Sources

- [[lecture-bs-5]] (p19)
