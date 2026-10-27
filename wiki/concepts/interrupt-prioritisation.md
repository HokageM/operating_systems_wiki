---
title: Interrupt Prioritisation
type: concept
tags: [interrupts, scheduling, hardware]
chapter: "01"
sources: [lecture-bs-1]
created: 2026-09-30
updated: 2026-09-30
---

# Interrupt Prioritisation

## Definition

**Interrupt prioritisation** (Priorisierung von Interrupts) is the mechanism that decides which of
several simultaneously pending [[interrupt|interrupts]] is handled first, and whether a running
[[interrupt-service-routine]] may be preempted.

## Context

Belongs to [[interrupts-and-io]]. It is the reason ISRs must be kept short: while interrupts are
masked, requests can be suppressed or lost.

## How it works

### Two solutions

| | **Software solution** | **Hardware solution** |
|---|---|---|
| Mechanism | The CPU has only one interrupt input and **polls the devices in a fixed order** (Reihenfolge) | A **prioritisation circuit** (Priorisierungsschaltung) assigns a priority to each device and always forwards only the most urgent request |
| Priorities | Implicitly fixed by polling order | **Fixed** (feste) or **variable** priorities |
| Cost | No extra hardware, but CPU time per interrupt | Extra circuitry, but fast |

### Nested / staged interrupts (gestufte Interrupts)

- **Sequential processing** (sequentielle Verarbeitung) — interrupt processing is *disabled inside
  the ISR*; interrupts are handled strictly one after another.
- **Priority-controlled processing** (prioritätengesteuerte Verarbeitung) — interrupts may only be
  interrupted by interrupts of **higher** priority.

## Related

- [[interrupt]] — what is being prioritised.
- [[interrupt-service-routine]] — must be short so the masking window stays small.
- [[scheduler]] — the analogous decision maker one level up, for [[process|processes]].

## Common confusions

- **Prioritisation ≠ nesting.** Prioritisation says *who goes first*; nesting says *whether a
  handler can be interrupted at all*. Exams often ask for both.
- **Fixed vs. variable priorities is a property of the hardware solution**, not an independent
  third option.

## Sources

- [[lecture-bs-1]] (p05)
