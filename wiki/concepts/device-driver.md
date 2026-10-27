---
title: Device Driver
type: concept
tags: [io, kernel, drivers]
chapter: "01"
sources: [lecture-bs-1]
created: 2026-09-30
updated: 2026-09-30
---

# Device Driver

## Definition

A **driver** (Treiber) is a component **in the [[kernel]] that has access to the device registers
(Geräte-Register) and hides the peculiarities of the device from the other components** of the
operating system.

## Context

Belongs to [[operating-system-basics]] and [[interrupts-and-io]]. Drivers are how the OS turns a
piece of hardware into a usable [[resource]] and how it delivers the
[[virtual-machine|virtual machine]] promise for I/O.

## How it works

A driver has two duties:

1. it provides access to the periphery (the device side), and
2. it provides an interface for embedding the driver into the kernel (the OS side).

The OS side can be designed in two ways:

| | **Device-dependent interface** (geräteabhängig) | **Device-independent interface** (geräteunabhängig) |
|---|---|---|
| Capability | Allows full exploitation of all device properties | Uniform I/O interface: `open()`, `read()`, ... |
| Cost | The I/O system must be extended for every driver | — |
| Extensibility | — | Guarantees easy extensibility, independent of existing applications |
| Virtual devices | Not possible | Allowed |

Device interrupts arrive at the driver's [[interrupt-service-routine]].

## Related

- [[kernel]] — drivers live inside it.
- [[virtual-machine]] — device-independent interfaces are one instance of the abstraction.
- [[interrupt-service-routine]] — the driver's handler half.
- [[direct-memory-access]] — what a driver programs for bulk transfer.

## Common confusions

- **"Device-independent" does not mean the driver is generic** — the driver is still
  device-specific; it is the *interface it exposes upward* that is uniform.
- The two interface styles are a trade-off, not a right/wrong choice — see
  [[operating-system-basics]].

## Sources

- [[lecture-bs-1]] (p02)
