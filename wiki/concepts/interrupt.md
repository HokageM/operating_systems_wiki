---
title: Interrupt
type: concept
tags: [interrupts, hardware]
chapter: "01"
sources: [lecture-bs-1, lecture-bs-5]
created: 2026-09-30
updated: 2026-09-30
---

# Interrupt

## Definition

An **interrupt** is a **signal that interrupts the normal execution of a program in order to carry
out a different processing**. Interrupts improve the instruction throughput of processors.

By trigger:

- triggered by **hardware** → **not reproducible and unpredictable** (the interrupt proper);
- triggered by an **instruction** → **predictable and reproducible** → this is a [[trap]].

## Context

The core of [[interrupts-and-io]]. Without interrupts the CPU would have to poll every device
(programmed I/O) and could never be reclaimed from a running [[process]].

## How it works

1. The device raises the interrupt signal.
2. The current program is interrupted; its state is saved by the glue routine — a
   [[context-switch]].
3. The interrupt number indexes the [[interrupt-vector-table]], yielding the start address of the
   associated [[interrupt-service-routine]].
4. The ISR runs, then the saved state is restored and the interrupted program continues.

Handling model: an interrupt **must** be handled by *resumption* (Wiederaufnahme) and **must
never** be handled by *termination* (Beendigung) — killing the running process would be arbitrary,
since the interrupt has nothing to do with it. Compare [[trap]], which may use either.

When several devices compete, [[interrupt-prioritisation]] decides who is served first, and nested
interrupts are processed either sequentially or priority-controlled.

## Related

- [[trap]] — the synchronous, instruction-triggered sibling.
- [[interrupt-service-routine]] — the code that runs.
- [[interrupt-vector-table]] — the dispatch table.
- [[context-switch]] — the save/restore around it.
- [[process-switch]] — what an interrupt is *not*: an interrupt alone causes only a context switch.
- [[page-fault]] — the [[memory-management-unit]] signals a missing page by raising an interrupt;
  afterwards the **last instruction is repeated**, which is the one case where an interrupted
  instruction is re-executed rather than resumed.
- [[clock-interrupt]] — the periodic interrupt the OS raises against itself; the basis of all time-slice based [[scheduling]].

## Common confusions

- **Interrupt vs. trap** — the classic exam distinction; see the reproducible/predictable axis
  above.
- **An interrupt does not imply a process switch.** Only a context switch happens; a
  [[process-switch]] occurs only if the [[scheduler]] decides so.
- **"Improves throughput" is the lecture's phrasing** — the benefit is CPU utilisation, not lower
  latency for the interrupted program.

## Sources

- [[lecture-bs-1]] (p03–p05)
- [[lecture-bs-5]] (p02, p06) — the clock interrupt as the enabler of preemption.
