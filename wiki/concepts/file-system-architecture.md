---
title: File System Architecture
type: concept
tags: [file-system, storage]
chapter: "05"
sources: [lecture-bs-5]
created: 2026-09-30
updated: 2026-09-30
---

# File System Architecture

## Definition

The layered software structure of the [[file-management-system]], from the application's API down
to the hardware.

## The layers

From the top down:

| Layer | Responsibility |
|---|---|
| **access methods** (Zugriffsmethoden) | the **API** the application calls |
| **logical I/O** | files as logical objects: names, records, offsets |
| **I/O supervisor** | **assignment of files to devices**; **scheduling of accesses**; **I/O buffers** |
| **simple file system** | **management of data blocks**; **block transfer** |
| **[[device-driver]]** | the peculiarities of one concrete device |

Each layer speaks in a coarser unit than the one below it: the driver in device commands, the
simple file system in [[disk-block|blocks]], logical I/O in files and offsets, the access methods
in application-level operations.

## Criteria for file organisation and access

The five criteria given alongside the architecture (p12), which every design decision in the
chapter trades against:

1. **fast access**
2. **easy updating**
3. **economical use of storage**
4. **simple administration**
5. **reliability**

## Context

Belongs to [[file-systems]]. The **I/O supervisor** layer is where the concerns of earlier chapters
reappear: *scheduling of accesses* is [[scheduling]] applied to disk requests, and *I/O buffers*
are [[cache|caching]] applied to blocks. *Assignment of files to devices* is what makes
[[mounting]] possible.

## Related

- [[file-management-system]] — the component this is the architecture of.
- [[device-driver]] — the bottom layer.
- [[disk-block]] — the unit of the simple file system.
- [[interrupts-and-io]] — how the driver actually talks to the hardware; see
  [[direct-memory-access]].
- [[kernel]] — all of these layers live inside it.
- [[locality]] — the justification for the I/O buffers.

## Exam relevance

Layer lists are cheap definition questions. Note that the I/O supervisor carries **three** named
duties — dropping one loses marks.

> [!question] Unverified — not in the notes
> Disk-access scheduling is named here as a duty of the I/O supervisor, but no algorithm for it is
> given anywhere in the notes. See the open question in [[file-systems]].

## Sources

- [[lecture-bs-5]] (p12)
