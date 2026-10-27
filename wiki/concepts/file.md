---
title: File
type: concept
tags: [file-system, storage]
chapter: "05"
sources: [lecture-bs-5]
created: 2026-09-30
updated: 2026-09-30
---

# File

## Definition

A **file** (Datei) is a **logical unit for storing information on external media**, having a
**name** and **administration information** (Verwaltungsinformationen).

Three parts, all of which belong in the exam answer: it is *logical* (not a physical region), it
has a *name* (so it can be referred to without knowing where it is), and it carries
*administration information* — see [[file-metadata]].

## Context

The central abstraction of [[file-systems]]. It stands in the same relation to a
[[disk-block]] as a process's virtual address space does to a [[page-and-frame|frame]]: a
contiguous, named, growable thing built out of scattered fixed-size pieces.

## Structure

The notes distinguish two forms — see [[file-structures]]:

- **unstructured**: a sequence of bytes, interpreted only by the application.
- **structured**: a sequence of fixed-length data records.

## Related

- [[file-management-system]] — what implements files.
- [[file-metadata]] — the administration information in the definition.
- [[directory]] — what gives files their place in a name space.
- [[file-access-types]], [[memory-mapped-file]], [[asynchronous-file-access]] — how files are read
  and written.
- [[i-node]], [[contiguous-allocation]], [[linked-list-allocation]],
  [[file-allocation-table]] — how a file's blocks are found.
- [[sparse-file]] — a file whose logical size exceeds the storage it occupies.
- [[pipe]] — in UNIX presented through the same interface, but without persistent storage.

## Common confusions

- **A file is not its blocks.** The mapping from the file's logical block numbers to physical
  blocks is the job of an allocation method, and that mapping changes over the file's lifetime.
- **A file's logical size is not the space it occupies.** A 6 KiB file in a 4 KiB-block system
  occupies 8 KiB ([[fragmentation|internal fragmentation]]); a [[sparse-file]] occupies far less
  than its size.

## Sources

- [[lecture-bs-5]] (p11)
