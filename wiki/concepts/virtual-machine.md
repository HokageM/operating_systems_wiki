---
title: Virtual Machine
type: concept
tags: [basics, abstraction]
chapter: "01"
sources: [lecture-bs-1]
created: 2026-09-30
updated: 2026-09-30
---

# Virtual Machine

## Definition

A **virtual machine** (virtuelle Maschine, VM) is an **abstraction of the real hardware**:
software that enables the use of the real hardware through a simple interface.

Offering a virtual machine to the users of the computing system is the third clause of the
lecture's definition of an [[operating-system]].

## Context

Belongs to [[operating-system-basics]]. It is the *convenience* goal (Bequemlichkeit) made
concrete.

## How it works

Instead of programming device registers, an application calls uniform operations such as `open()`
and `read()` — see [[device-driver]]'s device-independent interfaces, which explicitly "allow
virtual devices". The same idea recurs at every level:

- a [[process]] gets its own **virtual processor**, producing pseudo-parallel execution;
- a process gets its own private address space, i.e. virtual memory;
- a file is a virtual, uniformly addressable device.

## Related

- [[operating-system]] — provides the VM.
- [[device-driver]] — the mechanism that makes device-independent interfaces possible.
- [[process]] — each one experiences its own virtual processor and address space.

## Common confusions

- **Not a hypervisor.** In this lecture "virtuelle Maschine" means the *abstraction layer the OS
  presents to programs*, not VirtualBox/VMware-style full machine virtualisation.
- **Not the same as an emulator** — no foreign instruction set is being simulated.

## Sources

- [[lecture-bs-1]] (p01)
