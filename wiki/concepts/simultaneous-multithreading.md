---
title: Simultaneous Multithreading (SMT)
type: concept
tags: [thread, hardware, cpu, parallelism]
chapter: "02"
sources: [lecture-bs-2]
created: 2026-09-30
updated: 2026-09-30
---

# Simultaneous Multithreading (SMT)

## Definition

**Simultaneous Multithreading** is **hardware-supported multithreading in which one physical
processor is split into two logical processors**. A **complex control unit (Steuerwerk)** and
**two complete register sets** allow the execution of **two parallel instruction streams**
(Befehlsströme).

## Context

Belongs to [[threads]]. It is the *hardware* side of the chapter; everything else in the chapter
is an OS-level construct.

## How it works

The two ingredients are stated explicitly and both are needed:

1. **Two complete register sets** — so two architectural states exist at once and no saving or
   restoring ([[thread-switch]]) is required to alternate between them.
2. **A complex control unit** — which issues instructions from both streams into the shared
   execution units.

The operating system sees **two logical processors** and schedules onto them as if they were two
CPUs. Each [[thread]] still has its own stack, as noted alongside the definition.

## Related

- [[thread]] — what gets executed on the logical processors.
- [[thread-switch]] — what SMT makes unnecessary between the two resident streams.
- [[processor-registers]] — duplicated here; in the single-threaded case they are saved and
  restored instead.
- [[cpu-utilisation]] — the metric SMT exists to improve.

## Common confusions

- **Logical ≠ physical.** There is still one physical processor: one set of execution units, one
  cache. Two logical processors do not give double the throughput.
- **SMT is hardware, threads are software.** A program can use [[thread|threads]] on a processor
  without SMT, and SMT works without the program knowing.
- **It is not a dual-core CPU.** Cores duplicate execution units; SMT duplicates only the
  register sets and widens the control unit.

## Sources

- [[lecture-bs-2]] (p03)
