---
title: Operating System
type: concept
tags: [basics, definition]
chapter: "01"
sources: [lecture-bs-1, lecture-bs-5]
created: 2026-09-30
updated: 2026-09-30
---

# Operating System

## Definition

An **operating system** (Betriebssystem, BS) is software which

1. manages all [[resource|Betriebsmittel]] of a computing system,
2. controls and accounts for their allocation (deren Zuteilung kontrolliert), and
3. offers the users of the computing system a [[virtual-machine|virtual machine]].

A shorter formulation from the same page: programs that manage the resources of a computer and
make them available to the application programs.

Its three goals (Ziele) are **convenience** (Bequemlichkeit), **efficiency** (Effizienz) and
**adaptability** (Anpassbarkeit).

## Context

The foundational definition of [[operating-system-basics]]. Everything else in the module is an
elaboration of one of the three clauses: resource management (memory, CPU, devices), allocation
control ([[scheduler]], protection, [[user-mode-and-kernel-mode]]) and virtualisation
([[virtual-machine]], [[device-driver|device-independent interfaces]], [[process]]).

## How it works

The OS sits as a layer between hardware and applications:

```
User ↕ Anwendung ↕ BS ↕ HW
```

Its **tasks** (Aufgaben) are: execution of application programs; protection of concurrently
executed programs; data exchange with devices; multi-user operation (Mehrbenutzerbetrieb); error
detection and reporting; [[process|memory management]] (Speicherverwaltung).

Its central component is the [[kernel]].

## Related

- [[kernel]] — the OS *core*; the OS as a whole also includes user interfaces and utilities that
  run outside the kernel.
- [[virtual-machine]] — one of the three things the OS *provides*, not a synonym for it.
- [[resource]] — the things it manages.
- [[file-management-system]] — the OS component that manages persistent storage, with transparency as its stated goal.
- [[scheduling]] — the OS component that manages the CPU.

## Common confusions

- **OS ≠ kernel.** The [[kernel]] is the lowest software layer with direct hardware access; the OS
  is the broader collection of programs.
- **"Manages resources" is not enough for full marks** — the lecture definition has three clauses,
  and the virtual machine clause is the one most often forgotten.

## Sources

- [[lecture-bs-1]] (p01)
- [[lecture-bs-5]] (p01, p11) — scheduling and file management as OS responsibilities.
