---
title: Wear Levelling
type: concept
tags: [file-system, storage, hardware]
chapter: "05"
sources: [lecture-bs-5]
created: 2026-09-30
updated: 2026-09-30
---

# Wear Levelling

## Definition

The function by which the **controller of a [[solid-state-disk]] distributes write accesses over
all cells** (Verteilung der Schreibzugriffe über alle Zellen), rather than repeatedly writing the
same physical cells.

## Context

Belongs to [[file-systems]]. Flash cells tolerate only a limited number of write cycles. Without
wear levelling, a frequently rewritten block — a [[file-allocation-table]], a directory, a
[[journaling-file-system|journal]] — would destroy its cells long before the rest of the device
aged at all, and the device would fail at a fraction of its potential lifetime.

## How it works

The controller keeps an indirection between the **logical block address** the operating system
uses and the **physical cell** the data actually occupies. On each write it picks a fresh,
little-used physical location and updates the mapping. Logically nothing changes; physically the
data moves constantly.

## Related

- [[solid-state-disk]] — where it runs.
- [[address-translation]] — structurally the same trick: an indirection layer that lets the
  physical location change while the logical name stays fixed. [[relocation]] does it for
  processes, the [[memory-management-unit]] for pages, wear levelling for flash blocks.
- [[disk-fragmentation]] — wear levelling deliberately *creates* physical scattering, which is
  harmless because an SSD has no seek time.
- [[journaling-file-system]] — a heavy repeat-writer, and therefore one of the reasons wear
  levelling is needed.

## Common confusions

- Wear levelling is done **by the device controller**, not by the operating system. The file system
  is unaware of it — another instance of the transparency goal of the
  [[file-management-system]].

## Sources

- [[lecture-bs-5]] (p10)
