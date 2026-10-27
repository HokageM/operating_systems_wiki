---
title: Memory Hierarchy
type: concept
tags: [memory, hardware, caching]
chapter: "04"
sources: [lecture-bs-4, lecture-bs-5]
created: 2026-09-30
updated: 2026-09-30
---

# Memory Hierarchy

## Definition

The **memory hierarchy** (Speicherhierarchie) is the ordering of a machine's storage levels seen
**from the CPU downwards**. Going down the hierarchy:

- **access speed decreases**
- **cost per bit decreases**
- **capacity increases**
- **access time (latency) increases**

Typical order: [[processor-registers]] → [[cache|caches (L1, L2, L3)]] → main memory →
persistent storage (SSD/HDD, including the swap area) → tertiary/archive storage.

## Context

Belongs to [[memory-management]]. The hierarchy only works because of [[locality]] — without it,
a small fast level would never contain what the CPU asks for next.

## How it works

Each level acts as a cache for the level below it. The OS and the hardware split the
responsibility:

| Boundary | Managed by |
|---|---|
| registers ↔ cache | hardware / compiler |
| cache ↔ main memory | hardware ([[cache]] controller) |
| main memory ↔ disk | the OS ([[virtual-memory]], [[page-fault]] handling) |

### Units used in the lecture

```
1 KiB = 2^10 B = 1024 B
1 MiB = 2^20 B
1 GiB = 2^30 B
```

These are the binary prefixes — the whole chapter's arithmetic is done in powers of two. See
[[address-width-calculation]].

## Related

- [[memory-technologies]] — *why* the levels differ: SRAM vs. DRAM vs. ROM.
- [[cache]] — the level the hardware manages.
- [[virtual-memory]] — the level the OS manages.
- [[locality]] — the empirical property that makes the hierarchy pay off.
- [[volatile-and-persistent-memory]] — the line that cuts the hierarchy in two.
- [[solid-state-disk]] — a level between main memory and the mechanical disk.
- [[disk-access-time]] — the measurement of its slowest level.

## Common confusions

- **The direction of the trade-off.** Exam wording is "from the CPU's point of view": sinking
  access speed *and* sinking cost, rising capacity *and* rising access time. Getting the sign of
  cost wrong is the classic mistake — the *fast* levels are the *expensive* ones.
- **KiB vs. kB.** The lecture uses binary prefixes throughout (2^10, not 10^3).

## Sources

- [[lecture-bs-4]] (p01, p18)
- [[lecture-bs-5]] (p10, exercise S-4) — the persistent levels of the hierarchy, quantified.
