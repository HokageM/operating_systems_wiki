---
title: Relocation
type: concept
tags: [memory, addressing]
chapter: "04"
sources: [lecture-bs-4]
created: 2026-09-30
updated: 2026-09-30
---

# Relocation

## Definition

**Relocation** (Relokation) is the property that a [[process]] **can occupy different memory areas
at different points in time**. The area a process lives in is not fixed for its lifetime: it may
be moved during compaction, swapped out and reloaded elsewhere (see [[swapping]]), or — under
[[paging]] — scattered over frames that change between runs.

## Context

Belongs to [[memory-management]]. Relocation is what forces the separation of
[[physical-and-virtual-memory]]: if a process may move, its program may not contain physical
addresses, so addresses must be translated at access time — see [[address-translation]] and the
[[memory-management-unit]].

## How it works

A program is compiled against **virtual (logical) addresses** starting at 0. At runtime the
hardware adds the current location:

- Under [[static-partitioning]] or [[dynamic-partitioning]], the current location is a single
  number held in the [[base-and-limit-register|base register]] — moving a process means changing
  one register.
- Under [[paging]], the location is the set of frame numbers in the [[page-table]] — moving a
  process means rewriting page-table entries; the process's own addresses never change.

Because the translation happens on every single access, it must be extremely efficient, which is
why it is done in hardware.

## Related

- [[physical-and-virtual-memory]] — the distinction relocation makes necessary.
- [[address-translation]] — the mechanism.
- [[dynamic-partitioning]] — one of its runtime strategies is to relocate other processes to make
  a hole large enough.
- [[swapping]] — a swapped-out process is typically reloaded somewhere else.

## Common confusions

- **Relocation is not the same as swapping.** Swapping moves a process *out of* memory;
  relocation is the weaker statement that its *position within* memory may change.
- **It concerns positions, not sizes.** Whether a process may *grow* is a separate question
  ("room to grow", see [[dynamic-partitioning]]).

## Sources

- [[lecture-bs-4]] (p03)
