---
title: Directory
type: concept
tags: [file-system, storage]
chapter: "05"
sources: [lecture-bs-5]
created: 2026-09-30
updated: 2026-09-30
---

# Directory

## Definition

A **directory** (Verzeichnis) **structures all files of a file system in a hierarchy**.

## Context

Belongs to [[file-systems]]. The [[file]] definition gives each file a *name*; the directory gives
that name a *place*, turning a flat set of names into a tree in which the same short name may occur
many times without ambiguity.

## How it works

A directory is itself stored as a file, whose content is a list of entries. Each entry associates a
name with the information needed to find the file — in an [[i-node]] system the i-node number, in a
[[file-allocation-table|FAT]] system the number of the first block plus the
[[file-metadata|attributes]]. Because a directory is a file, directories can contain directories,
and the hierarchy follows.

## Related

- [[file]] — what is being organised.
- [[file-metadata]] — often held in, or reached through, the directory entry.
- [[mounting]] — how several independent directory trees become one.
- [[partition]] — each holds exactly one file system, and therefore its own root directory.
- [[i-node]] — the directory entry stores the i-node number, not the blocks.
- [[multi-level-page-table]] — the same structural idea in memory management: a tree of index
  structures instead of one flat table, so that only the populated parts exist.

## Common confusions

- **The directory does not contain the files.** It contains *references*. That is what makes it
  possible for two directory entries to name the same file, and why deleting a name is not
  necessarily deleting the data.

## Sources

- [[lecture-bs-5]] (p14)
