---
title: File Management System
type: concept
tags: [file-system, storage]
chapter: "05"
sources: [lecture-bs-5]
created: 2026-09-30
updated: 2026-09-30
---

# File Management System

## Definition

The operating-system component that **provides services for using [[file|files]]** and
**unifies persistent storage towards the applications**.

Its stated **goal: transparency of how and where storage works** — the application should not need
to know what medium it is on, where the blocks lie, or how they are chained.

## Its tasks

The list from p11, worth memorising as a list:

1. **An API for files** — the interface through which applications work.
2. **Integrity** — the data read back is the data written.
3. **Support I/O with many different storage devices.**
4. **Coordinate simultaneous I/O requests.**
5. **Optimise performance.**

Tasks 3 and 4 are the resource-management side (the same job the
[[operating-system]] does for every [[resource]]); tasks 1 and 2 are the abstraction side.

## Context

Belongs to [[file-systems]]. It is the direct counterpart of the [[memory-management-unit]] plus
the memory manager one layer up: both exist to give a uniform, protected, location-independent view
of a physical storage device. The structure through which it does so is described in
[[file-system-architecture]].

## Related

- [[file-system-architecture]] — the five layers it is built from.
- [[file]] — what it manages.
- [[device-driver]] — its lowest layer, and the place where device peculiarities are hidden.
- [[mounting]] — how it presents several devices as one name space.
- [[journaling-file-system]] — how it keeps its integrity promise across crashes.
- [[operating-system]] — "manages resources, controls allocation, offers a virtual machine" applies
  verbatim here.
- [[virtual-machine]] — a file *is* a virtual storage device.

## Common confusions

- **File management system vs. file system.** In these notes the *file management system* is the
  OS component (the software), while *a file system* (as in "every [[partition]] holds exactly one
  file system") is the on-disk structure it manages. Both are called "Dateisystem" in German, so
  read from context.

## Sources

- [[lecture-bs-5]] (p11)
