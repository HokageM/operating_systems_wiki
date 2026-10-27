---
title: Base and Limit Register
type: concept
tags: [memory, protection, hardware]
chapter: "04"
sources: [lecture-bs-4]
created: 2026-09-30
updated: 2026-09-30
---

# Base and Limit Register

## Definition

A pair of hardware registers that implement **memory protection** under partitioning:

- the **base register** (Basisregister) holds the **start address** of the area assigned to the
  running process;
- the **limit register** (Grenzregister) holds its **end address**.

Every address the process generates is checked against the pair; an address outside the interval
is an access violation and traps into the operating system.

## Context

Belongs to [[memory-management]]. It is the protection mechanism of [[static-partitioning]] and
[[dynamic-partitioning]], and the direct ancestor of the far more general protection bits in a
[[page-table-entry]].

## How it works

```
physical address = base + virtual address
valid  <=>  base <= physical address <= limit
```

One addition and one comparison per access — which is why partitioning can claim "(+) simple
address translation" (see [[address-translation]]).

The register pair belongs to the process's machine state and must be reloaded on every
[[process-switch]]. Because only the [[kernel]] may write these registers (they are
privileged, see [[user-mode-and-kernel-mode]]), a process cannot escape its own area.

## Related

- [[static-partitioning]] / [[dynamic-partitioning]] — the schemes that use it.
- [[memory-management-unit]] — its successor, generalised to page-wise translation and protection.
- [[relocation]] — moving a process means changing only the base register.
- [[user-mode-and-kernel-mode]] — why the registers themselves are safe.

## Common confusions

- **Some textbooks make the second register a *length* rather than an end address.** The lecture
  notes define the Grenzregister as the **end address** of the area; check the wording of the
  exam question before computing.
- **Protection and translation are the same mechanism here.** Under partitioning one register
  pair does both jobs; under [[paging]] they separate into frame numbers (translation) and
  protection bits (protection).

## Sources

- [[lecture-bs-4]] (p04)
