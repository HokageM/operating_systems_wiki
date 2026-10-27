---
title: Mounting
type: concept
tags: [file-system, storage]
chapter: "05"
sources: [lecture-bs-5]
created: 2026-09-30
updated: 2026-09-30
---

# Mounting

## Definition

The technique of **mountable directory trees** (einhängbare Verzeichnisbäume): the
[[directory]] tree of one storage device is attached at a chosen point inside the tree of another,
so that **after mounting a single file-name tree exists**.

## The two alternatives

The notes present mounting as one of two ways to handle several storage devices:

| Alternative | Idea | Example |
|---|---|---|
| **1. names mirror the storages** | each device gets its own name-space root | `C:\`, `D:\` (Windows) |
| **2. transparency in the file name space** | the devices are **mounted** into one tree | `/`, `/home`, `/mnt/usb` (UNIX) |

Alternative 2 realises the **transparency goal** of the [[file-management-system]]: from the path
alone you cannot tell — and need not care — which physical device a file lives on.

## Context

Belongs to [[file-systems]]. Each mounted subtree is a separate file system on its own
[[partition]] or [[logical-volume-manager|logical volume]]; mounting is what hides that fact.

## Related

- [[directory]] — what is being attached.
- [[partition]] — "every partition holds exactly one file system", and each such file system is
  mountable.
- [[logical-volume-manager]] — solves the same problem at a lower level: combining several physical
  drives into one addressable volume rather than one name space.
- [[file-management-system]] — transparency is its stated goal.
- [[virtual-machine]] and [[virtual-memory]] — the same pattern: hide the physical arrangement
  behind a uniform logical one.

## Common confusions

- **Mounting vs. LVM.** Mounting unifies the **name space** and leaves the devices separate;
  an [[logical-volume-manager|LVM]] unifies the **storage** so that one file system can span
  several disks. They are orthogonal and often used together.
- A mount point's original content is hidden, not deleted, while something is mounted over it.

## Sources

- [[lecture-bs-5]] (p14)
