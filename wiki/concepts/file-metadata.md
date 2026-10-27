---
title: File Metadata
type: concept
tags: [file-system, storage]
chapter: "05"
sources: [lecture-bs-5]
created: 2026-09-30
updated: 2026-09-30
---

# File Metadata

## Definition

**Metadata** is the administration information belonging to a [[file]] rather than its content —
the **file attributes**: access **rights**, **type**, **size**, and further entries such as
timestamps and the owner.

## Context

Belongs to [[file-systems]]. Metadata is the "administration information" named in the very
definition of a [[file]]. Where it is stored depends on the allocation method: in an
[[i-node]]-based system it is the first part of the i-node; in a
[[file-allocation-table|FAT]]-based system it sits in the directory entry.

## Related

- [[file]] — what the metadata describes.
- [[directory]] — the structure that holds or points to it.
- [[i-node]] — "a file control block containing attributes plus a table of block addresses"; the
  attributes are exactly this metadata.
- [[process-control-block]] — the direct analogue for processes: all administration information
  about one object, separate from the object's own data. The parallel is deliberate and the notes
  even use the phrase *file control block*.
- [[journaling-file-system]] — metadata is the part most journalling schemes protect first, because
  inconsistent metadata loses whole files, not just bytes.

## Common confusions

- **Metadata is not stored in the file.** Reading a file returns its content only; the size, rights
  and timestamps come from elsewhere. This is why a [[sparse-file]] can report a size far larger
  than the blocks it occupies.

## Open questions

> [!question] Unverified — not in the notes
> Access rights are listed as an attribute but never developed: no user/group/other model, no
> permission bits, no ACLs. If the exam asks about protection, the notes offer only
> [[user-mode-and-kernel-mode]] and [[base-and-limit-register]] from earlier chapters.

## Sources

- [[lecture-bs-5]] (p14)
