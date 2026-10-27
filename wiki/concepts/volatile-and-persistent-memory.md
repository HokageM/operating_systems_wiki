---
title: Volatile and Persistent Memory
type: concept
tags: [file-system, storage, memory]
chapter: "05"
sources: [lecture-bs-5]
created: 2026-09-30
updated: 2026-09-30
---

# Volatile and Persistent Memory

## Definition

- **Volatile memory** (flüchtiger Speicher) — requires a **permanent energy supply**; otherwise the
  data is lost. Example: **RAM**.
- **Persistent memory** (persistenter Speicher) — stores data **durably without energy**. Example:
  **flash**.

## Context

Belongs to [[file-systems]]. This single distinction is the reason file systems exist at all:
everything a program computes lives in volatile memory, and anything that must survive the power
being cut has to be moved to a persistent medium.

It also cuts the [[memory-hierarchy]] in two. Registers, [[cache]] and main memory are volatile;
disks, SSDs and tapes are persistent. The boundary is exactly where the units change from bytes to
**[[disk-block|blocks]]** and the latency jumps by orders of magnitude.

## Related

- [[memory-hierarchy]] — the volatile/persistent line runs through it.
- [[memory-technologies]] — RAM, ROM, SRAM, DRAM: ROM is the persistent member of that family.
- [[solid-state-disk]] — the flash-based persistent medium of this chapter.
- [[swapping]] and [[virtual-memory]] — the one case where the *system* deliberately moves volatile
  data to persistent storage without the application asking.
- [[file]] — the abstraction over persistent storage.
- [[journaling-file-system]] — exists because persistence is not the same as consistency.

## Common confusions

- **Persistent ≠ reliable.** A persistent medium keeps data without power, but can still fail
  mechanically or electrically — which is why [[raid]] exists.
- **Persistent ≠ consistent.** If the power fails *during* a write, the data is durably stored in a
  half-written state. See [[journaling-file-system]] and
  [[raid-consistent-update-problem]].
- The German *flüchtig* ("fleeting") is the exam term; "non-volatile" and "persistent" are used
  interchangeably in the notes.

## Sources

- [[lecture-bs-5]] (p10)
