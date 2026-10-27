---
title: RAID Consistent Update Problem
type: concept
tags: [file-system, storage, raid, reliability]
chapter: "05"
sources: [lecture-bs-5]
created: 2026-09-30
updated: 2026-09-30
---

# RAID Consistent Update Problem

## Definition

A logical write to a [[raid]] array must be **atomic**: it must reach **all** the affected disks or
**none** of them. But the controller works **sequentially**, writing one disk after another.

**The problem:** a **power failure between the individual disk writes** leaves the array in a
state that is neither the old one nor the new one — data updated but parity not, or one mirror
updated and the other not. The redundancy is now silently *wrong*, and a later reconstruction will
produce garbage with full confidence.

## The solution: write-ahead log

**Writes first go into a battery-backed RAM, then to the disks.** After a power failure the
consistency is **restored from the log**: the controller replays the entries whose disk writes did
not complete.

The battery matters — the log must survive the same power failure it protects against.

## Context

Belongs to [[raid]]. It is the reason a hardware RAID controller has memory and a battery at all,
and it applies to every redundant level: [[raid-1]] (both mirrors), [[raid-4]], [[raid-5]] and
[[raid-6]] (data plus one or two parity blocks).

## Related

- [[journaling-file-system]] — **the identical technique one layer up.** A journal protects a
  multi-block file-system update; a write-ahead log protects a multi-disk RAID update. Same
  problem, same solution, different layer. Being able to state that parallel is worth marks.
- [[atomic-operation]] — the property being manufactured, defined back in chapter 03.
- [[parity-and-xor]], [[raid-parity-update]] — what is being kept consistent.
- [[deadlock-recovery]] — checkpoint and roll back: the same pattern for processes.
- [[write-through-and-write-back]] — a battery-backed write-back cache is what the log physically
  is.

## Common confusions

- **This is not the same as a disk failing.** RAID's redundancy protects against a disk failing;
  nothing in the redundancy scheme protects against the *update of the redundancy* being
  interrupted. That is a separate problem needing a separate mechanism — which is exactly why it
  gets its own name.

## Sources

- [[lecture-bs-5]] (p23)
