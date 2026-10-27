---
title: Journaling File System
type: concept
tags: [file-system, storage, reliability]
chapter: "05"
sources: [lecture-bs-5]
created: 2026-09-30
updated: 2026-09-30
---

# Journaling File System

## Definition

A file system in which **a consistent state can be reconstructed at any time, even if a write was
aborted**. Changes are **recorded before the actual write** in a reserved area, the **journal**.

## How it works

1. The intended change is written to the **journal** (in a reserved area of the medium).
2. Only then is the actual write to the file system performed.
3. On success the journal entry is discarded.

After a crash the file system replays the journal: entries whose real write did not complete are
either redone or rolled back, so the medium is returned to a consistent state without a full scan.

## Pros and cons

| | |
|---|---|
| (+) | **stays consistent after an aborted write** |
| (−) | **write operations take longer** — everything is written twice |

## Context

Belongs to [[file-systems]]. It addresses the gap that
[[volatile-and-persistent-memory|persistence alone leaves open]]: a persistent medium faithfully
preserves a half-finished update. The danger is greatest for operations that touch several blocks
at once — a [[directory]] entry plus an [[i-node]] plus a data block — where a crash in between
leaves a file that is referenced but has no content, or blocks that belong to nobody.

## Related

- [[raid-consistent-update-problem]] — **the identical technique one layer down**: a RAID
  controller's *write-ahead log* protects a multi-disk write the way a journal protects a
  multi-block write. Same problem, same solution, different layer.
- [[atomic-operation]] — the property being manufactured. A journal is how you build an atomic
  multi-block write out of non-atomic single-block writes.
- [[deadlock-recovery]] — the checkpoint-and-roll-back strategy of chapter 03 is the same idea
  applied to processes instead of to storage.
- [[write-through-and-write-back]] — the opposite trade: caching delays writes for speed,
  journalling duplicates them for safety.
- [[file-management-system]] — **integrity** is one of its five stated tasks; this is how it is
  kept.
- [[wear-levelling]] — the journal is written constantly, so on flash it needs special handling.

## Common confusions

- **Journalling protects consistency, not your data.** A crash may still lose the most recent
  writes; what it may not do is leave the file system structurally broken.
- Many journalling file systems journal **[[file-metadata|metadata]] only** by default, precisely
  because journalling everything doubles all write traffic.

## Sources

- [[lecture-bs-5]] (p19)
