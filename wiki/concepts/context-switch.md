---
title: Context Switch
type: concept
tags: [interrupts, process, switching]
chapter: "01"
sources: [lecture-bs-1, lecture-bs-5]
created: 2026-09-30
updated: 2026-09-30
---

# Context Switch

## Definition

A **context switch** (Kontextwechsel) is the **saving of the state of the currently executing
program and the later restoration of that state**, performed around the execution of an
[[interrupt-service-routine]] or on entry into the [[kernel]]. It does **not** by itself imply
that a different [[process]] is scheduled.

## Context

Belongs to [[interrupts-and-io]] and [[processes]]. It is the cheap sibling of the
[[process-switch]].

## How it works

The lecture's three-column diagram:

1. **Interrupted program** (unterbrochenes Programm) — state is automatically saved on entry and
   reconstructed (Rekonstruktion) on return.
2. **Glue routine** (Kopplungsroutine, written in assembler) —
   Zustandssicherung (state saving) → Aufruf (call) → Zustandswiederherstellung (state restore) →
   Rückkehr (return).
3. **Handler** (Unterbrechungsbehandlungsroutine, written in a high-level language) — the actual
   handling; entered via the ISR, left via `RTS`.

The compiler generates the machine instructions that save and restore the working
[[processor-registers]] and resume program execution.

## Related

- [[process-switch]] — the expensive, 8-step operation that *does* change the running process.
- [[interrupt-service-routine]] — what the switch wraps around.
- [[processor-registers]] — what is saved.
- [[system-call]] — causes a context switch into kernel mode.
- [[thread-switch]] — the scheduling-level switch that is cheap for the same reason: the address
  space does not change.
- [[spin-lock]] — the cost of this switch is the yardstick for the spin-versus-block decision:
  spinning wins when the expected wait is shorter than a context switch.
- [[mutex]] — blocking costs **two** switches, out and back in again.
- [[clock-interrupt]] — the interrupt that causes a context switch which then usually becomes a full [[process-switch]].
- [[preemptive-and-non-preemptive-scheduling]] — preemption always starts as a context switch.

## Common confusions

- **Context switch ≠ process switch.** This is an explicit exam point: on a [[system-call]], on a
  switch into kernel mode, and on an [[interrupt]], **no process switch takes place** — only a
  context switch.
- The glue routine and the handler are two different pieces of code, in two different languages,
  for a reason: saving registers cannot be expressed in a high-level language.

## Sources

- [[lecture-bs-1]] (p04, p16)
- [[lecture-bs-5]] (p02, p06) — the clock interrupt path into the scheduler.
