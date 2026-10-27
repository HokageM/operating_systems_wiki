---
title: Memory Technologies (RAM, ROM, SRAM, DRAM)
type: concept
tags: [memory, hardware]
chapter: "04"
sources: [lecture-bs-4]
created: 2026-09-30
updated: 2026-09-30
---

# Memory Technologies (RAM, ROM, SRAM, DRAM)

## Definition

The physical building blocks of the [[memory-hierarchy]]:

| | Persistence | Where used | Speed | Power | Cost | Space needed |
|---|---|---|---|---|---|---|
| **RAM** (Random Access Memory) | **volatile** — contents lost without power | main memory, caches | — | — | — | — |
| **ROM** (Read Only Memory) | **persistent** | firmware, [[bios]] | — | — | — | — |
| **SRAM** (static RAM) | volatile | **caches** | very fast | low | expensive | high space consumption |
| **DRAM** (dynamic RAM) | volatile | **main memory** | slower | higher | cheaper | smaller |

SRAM and DRAM are both RAM; they differ in the cell design, and that single difference explains
the whole cache/main-memory split.

## Context

Belongs to [[memory-management]]. This is the hardware answer to the question the
[[memory-hierarchy]] poses: why not build everything out of the fast stuff?

## How it works

- **SRAM** stores a bit in a flip-flop. It needs no refresh, so it is fast and draws little power
  — but a flip-flop costs several transistors per bit, which makes it expensive and physically
  large. That limits it to small capacities: [[cache|caches]].
- **DRAM** stores a bit as a charge in a capacitor. One transistor per bit means high density and
  low cost, so it scales to gigabytes — but the charge leaks, so it must be refreshed
  periodically, which costs time and power. That makes it main memory.
- **ROM** keeps its contents without power, which is why the boot code lives there
  (see [[booting]]).

## Related

- [[memory-hierarchy]] — the ordering these technologies populate.
- [[cache]] — built from SRAM.
- [[bios]] — the classic ROM resident.

## Common confusions

- **"RAM" does not mean "main memory".** Caches are RAM too. RAM is about *random access* and
  volatility, not about position in the hierarchy.
- **SRAM is not "the small one because it is slow"** — it is small because it is *expensive and
  space-hungry*, despite being the fast one.

## Sources

- [[lecture-bs-4]] (p01)
