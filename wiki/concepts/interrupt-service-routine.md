---
title: Interrupt Service Routine
type: concept
tags: [interrupts, handler]
chapter: "01"
sources: [lecture-bs-1]
created: 2026-09-30
updated: 2026-09-30
---

# Interrupt Service Routine

## Definition

An **interrupt handler** or **interrupt service routine (ISR)**, German
*Unterbrechungsbehandlungsroutine*, is the program which, **on the occasion of an [[interrupt]],
executes the associated processing**.

## Context

Belongs to [[interrupts-and-io]]. Each interrupt type has its ISR; the mapping is held in the
[[interrupt-vector-table]].

## How it works

On an interrupt the current program is interrupted and the ISR is called. The lecture's
three-column picture splits the work:

| Interrupted program | Glue routine (Kopplungsroutine, assembler) | Handler (high-level language) |
|---|---|---|
| Automatic save on entry | Zustandssicherung (state saving) | |
| | Aufruf (call) → | Eigentliche Behandlung (the actual handling) |
| | ← Zustandswiederherstellung (state restore) | Return via `RTS` |
| Reconstruction (Rekonstrukt.) and continuation | Rückkehr (return) | |

The compiler generates the machine instructions that save and restore the working register
contents and resume program execution — this is the [[context-switch]].

**ISRs should be as short as possible**, to minimise the window in which further interrupts are
suppressed or masked.

## Related

- [[interrupt]] and [[trap]] — what invokes it.
- [[interrupt-vector-table]] — how it is found.
- [[context-switch]] — the save/restore wrapped around it.
- [[interrupt-prioritisation]] — whether an ISR may itself be interrupted.
- [[device-driver]] — most ISRs belong to a driver.

## Common confusions

- **The ISR is not the whole handling path.** The assembler glue routine that saves and restores
  state is a separate piece; exams may ask you to name both.
- **Short ISR ≠ incomplete handling** — long work is deferred, not skipped.
- An ISR run does **not** imply a [[process-switch]].

## Sources

- [[lecture-bs-1]] (p03–p05)
