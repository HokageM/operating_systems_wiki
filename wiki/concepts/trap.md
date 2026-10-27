---
title: Trap
type: concept
tags: [interrupts, traps, system-calls]
chapter: "01"
sources: [lecture-bs-1]
created: 2026-09-30
updated: 2026-09-30
---

# Trap

## Definition

A **trap** is an [[interrupt]] **triggered by an instruction** rather than by hardware.
Consequently it is **predictable and reproducible**: running the same program with the same data
raises the trap at the same point every time.

## Context

Belongs to [[interrupts-and-io]]. The trap is the mechanism behind the [[system-call]]: the user
program deposits parameters and a function ID in memory and *generates a trap* to enter the
[[kernel]].

## How it works

Traps share the whole interrupt apparatus — the [[interrupt-vector-table]], the
[[interrupt-service-routine]] and the [[context-switch]] glue routine. What differs is the
handling model:

| | [[interrupt]] | **Trap** |
|---|---|---|
| Trigger | Hardware | Instruction |
| Reproducible | No | Yes |
| Predictable | No | Yes |
| Resumption (Wiederaufnahme) | **must** | *may* |
| Termination (Beendigung) | **must never** | *may* |

A trap may be terminated because it *was* caused by the running program: a division by zero or an
illegal memory access can legitimately abort that process. An arithmetic error is listed under
involuntary process termination in [[processes]].

## Related

- [[interrupt]] — the general category.
- [[system-call]] — the deliberate, useful trap.
- [[user-mode-and-kernel-mode]] — what a trap flips (the mode bit).

## Common confusions

- **A trap is a kind of interrupt, not its opposite.** The lecture presents both as triggers of
  the same mechanism.
- **"May be resumed, may be terminated"** — the *may/may* row is what distinguishes it; writing
  "must be terminated" is wrong.
- Not every trap is an error: the [[system-call]] is a trap on purpose.

## Sources

- [[lecture-bs-1]] (p03, p04, p08)
