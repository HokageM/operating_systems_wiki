---
title: Disk Fragmentation
type: concept
tags: [file-system, storage]
chapter: "05"
sources: [lecture-bs-5]
created: 2026-09-30
updated: 2026-09-30
---

# Disk Fragmentation

## Definition

**Fragmentation of hard disks**: files which belong together contiguously are **scattered over the
whole disk** (zusammenhängende Dateien sind über die gesamte Festplatte verteilt).

## Context

Belongs to [[file-systems]]. It is the disk form of the external fragmentation defined in
[[fragmentation]] — free space exists, but not where it is needed, so a growing file has to take
whatever blocks are available.

## Why it hurts

Fragmentation converts **logically sequential access into physically random access**, and per
[[disk-access-time]] that is a factor of roughly 50 in effective throughput on a mechanical disk:
each scattered block costs a seek (7 ms) and a rotation (3 ms) instead of nothing at all.

It also defeats the reason for choosing a large [[disk-block]] size in the first place.

## Which allocation methods suffer

| Method | Fragmentation behaviour |
|---|---|
| [[contiguous-allocation]] | **fragments badly** — and compaction is *very expensive* |
| [[linked-list-allocation]] | no external fragmentation, but scatters freely and therefore **violates [[locality]]**; needs **periodic reorganisation** |
| [[file-allocation-table]] | same scattering, but the chain is walked in memory, so it costs less |
| [[i-node]] | same scattering; good allocators keep a file's blocks near its i-node |

## Related

- [[fragmentation]] — the general concept, internal and external.
- [[locality]] — what fragmentation destroys.
- [[disk-access-time]] — why destroying it is expensive.
- [[file-access-types]] — fragmentation only matters because sequential access is otherwise cheap.
- [[solid-state-disk]] — immune in practice: with no arm to move there is no seek penalty, so
  defragmenting an SSD only burns write cycles (see [[wear-levelling]]).

## Common confusions

- **Fragmentation of the disk vs. internal fragmentation of the last block.** The first is about
  *where* a file's blocks are; the second is about the unused remainder *inside* its final block.
  Both are called fragmentation and both appear in this chapter.

## Sources

- [[lecture-bs-5]] (p16)
