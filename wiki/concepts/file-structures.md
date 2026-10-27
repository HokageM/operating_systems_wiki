---
title: File Structures
type: concept
tags: [file-system, storage]
chapter: "05"
sources: [lecture-bs-5]
created: 2026-09-30
updated: 2026-09-30
---

# File Structures

## Definition

The two ways the content of a [[file]] can be organised:

- **Unstructured** (unstrukturiert) — a **sequence of bytes**, interpreted **only by the
  application**. The file system attaches no meaning to the content whatsoever.
- **Structured** (strukturiert) — a **sequence of fixed-length data records** (Datensätze
  fester Länge), which the file system itself understands.

## Context

Belongs to [[file-systems]]. The notes add the historical verdict: the structured form has been
**displaced by relational databases**. Record-level structure moved out of the operating system and
into a layer above it, leaving the file system with the simpler and more general byte-sequence
model.

## Why unstructured won

- **Generality.** A byte sequence can represent any structure; a fixed-record file cannot represent
  a byte sequence.
- **Separation of concerns.** The file system need not know about record formats, indexes or
  queries — a database can do all of that better, on top of plain files.
- It makes the file interface uniform with [[pipe|pipes]], devices and sockets: in UNIX everything
  is a byte stream.

## Related

- [[file]] — what is being structured.
- [[file-access-types]] — fixed-length records make **direct access** to record *n* trivial
  (multiply by the record size); with an unstructured file the application must know the offset
  itself.
- [[disk-block]] — the physical fixed-size unit, which exists regardless of the logical structure.
- [[file-management-system]] — the layer whose API this determines.

## Common confusions

- **Unstructured does not mean "no format".** A JPEG is an unstructured file with a very precise
  internal format — the point is that the *file system* does not know it.

## Sources

- [[lecture-bs-5]] (p11)
