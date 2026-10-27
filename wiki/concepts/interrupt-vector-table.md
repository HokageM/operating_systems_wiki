---
title: Interrupt Vector Table
type: concept
tags: [interrupts, dispatch]
chapter: "01"
sources: [lecture-bs-1]
created: 2026-09-30
updated: 2026-09-30
---

# Interrupt Vector Table

## Definition

The **interrupt vector table** (Interruptvektortabelle) maps each interrupt type to the **start
address of its [[interrupt-service-routine]]**.

## Context

Belongs to [[interrupts-and-io]]. It is the dispatch mechanism that makes interrupt handling O(1)
instead of a search.

## How it works

Each [[interrupt]] is assigned a **number**. That number is used as an **index into the table**;
the entry found there is the vector leading to the ISR. The CPU loads that address into the
program counter and the handler begins — after the glue routine has performed the
[[context-switch]].

```
interrupt number  →  [ vector table ]  →  ISR start address  →  handler runs
```

## Related

- [[interrupt]], [[trap]] — both are dispatched through the table.
- [[interrupt-service-routine]] — what the entries point at.
- [[system-call]] — reaches its handler through a trap vector.
- [[signal-handler]] — the notes use the same name for the **per-process** table that records how
  each [[signal]] is to be handled; do not confuse the two.

## Common confusions

- **The table stores addresses, not code.** An "interrupt vector" is a pointer.
- **Indexed, not searched** — the interrupt number *is* the index, which is why interrupt numbers
  are small dense integers.

## Sources

- [[lecture-bs-1]] (p03, p05)
