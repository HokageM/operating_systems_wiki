---
title: BIOS
type: concept
tags: [basics, startup, firmware]
chapter: "01"
sources: [lecture-bs-1]
created: 2026-09-30
updated: 2026-09-30
---

# BIOS

## Definition

**BIOS** = **Basic Input/Output System**. It is **firmware**: the software layer directly above
the hardware that **checks whether all software and hardware components required for the start are
ready for use, and loads the operating system into RAM**.

## Context

Belongs to [[operating-system-basics]]; it is the agent of [[booting]]. The layer sketch from the
notes:

```
BS (operating system)
BIOS
HW (hardware)
```

## How it works

On power-on the BIOS runs first, from non-volatile memory on the mainboard. It performs the
readiness check of the required components, then loads the [[operating-system]] into main memory
and transfers control to it. From that moment the [[kernel]] owns the hardware.

## Related

- [[booting]] — the process the BIOS performs.
- [[kernel]] — what the BIOS hands control to.
- [[device-driver]] — the OS's own, richer hardware access, which replaces BIOS routines once the
  OS is up.

## Common confusions

- **BIOS is firmware, not part of the OS.** It sits *below* the OS in the layer stack.
- It is not a [[virtual-machine]]: it does not stay in the path to abstract hardware for
  applications; after boot the OS takes over.

## Sources

- [[lecture-bs-1]] (p07)
