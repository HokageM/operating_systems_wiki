---
title: Thread Control Block (TCB)
type: concept
tags: [thread, kernel, data-structure]
chapter: "02"
sources: [lecture-bs-2]
created: 2026-09-30
updated: 2026-09-30
---

# Thread Control Block (TCB)

## Definition

The **Thread Control Block (TCB)** is the OS record that holds the state private to one
[[thread]]: its **program counter**, its **registers**, its **thread-local stack** and the
**pointer to its thread-local memory area**.

## Context

Belongs to [[threads]]. It is the per-thread analogue of the [[process-control-block]], and the
only thing that has to be exchanged on a [[thread-switch]].

## How it works

The notes split a running program's state into two buckets:

| Shared by all threads of a process | Private to a single thread |
|---|---|
| memory state (Speicherzustand) | program counter (Programmzähler) |
| file-system state (Dateisystemzustand) | registers |
| data, file table, interrupt-vector table | thread-local stack |
| code segment | pointer to thread-local memory area |

The left column stays in the [[process-control-block]] / [[process-image]]; the right column is
the TCB. Because the left column does not change when switching between two threads of the same
process, the switch reduces to *"exchange the TCB"*.

## Related

- [[process-control-block]] — the process-level counterpart; much larger.
- [[thread-switch]] — the operation that consumes it.
- [[thread-state-model]] — the state values it carries.
- [[thread]] — what it describes.

## Common confusions

- **The TCB is not a small PCB — it is a *different* set of fields.** Memory-management tables,
  the file table and the interrupt-vector table are deliberately *not* in it; that omission is the
  whole efficiency argument.
- The exam question *"Wodurch wird der Zustand eines Threads beschrieben?"* wants exactly the four
  private items, not the shared ones.

## Sources

- [[lecture-bs-2]] (p02, p05)
