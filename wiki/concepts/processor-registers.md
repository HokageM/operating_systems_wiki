---
title: Processor Registers
type: concept
tags: [hardware, registers, cpu]
chapter: "01"
sources: [lecture-bs-1]
created: 2026-09-30
updated: 2026-09-30
---

# Processor Registers

## Definition

The registers of the processor fall into two groups:

- **Control and status registers** (Kontroll- und Status-Register) — used by the processor itself;
  they serve the [[operating-system]] to control program execution. The program counter (PC) and
  the mode bit belong here.
- **User-visible registers** (benutzersichtbare Register) — the ALU processes data *in* these
  registers; data from main memory must first be copied into them. They are used within machine
  programs.

## Context

Belongs to [[operating-system-basics]] and [[interrupts-and-io]]. Registers are the fastest level
of the storage hierarchy and, crucially, the part of a process that must be **saved and restored**
on every switch.

## How it works

Both groups appear in the [[process-control-block]]: the PCB stores the contents of the
user-visible registers *and* the contents of the control and status registers (e.g. the PC). On an
[[interrupt]] the glue routine performs a [[context-switch]], saving the working registers so the
interrupted program can be resumed exactly where it stopped.

In [[user-mode-and-kernel-mode|user mode]] only a restricted set of machine instructions is
available and system data is not (or only read-) accessible; in kernel mode there is full access
to all registers.

## Related

- [[process-control-block]] — where register contents are parked while a process is not running.
- [[context-switch]] — the act of saving and restoring them.
- [[user-mode-and-kernel-mode]] — which registers a program may touch.
- [[thread-control-block]] — the per-thread copy of PC and registers.
- [[simultaneous-multithreading]] — duplicates the whole register set in hardware instead of
  saving and restoring it.

## Common confusions

- **"User-visible" does not mean "belonging to the user process only"** — it means visible to
  machine programs, as opposed to registers only the CPU/OS manipulates.
- The PC is a *control/status* register even though programs obviously depend on it.

## Sources

- [[lecture-bs-1]] (p03, p15)
