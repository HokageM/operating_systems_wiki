---
title: Booting
type: concept
tags: [basics, startup]
chapter: "01"
sources: [lecture-bs-1]
created: 2026-09-30
updated: 2026-09-30
---

# Booting

## Definition

**Booting** (Booten, Hochfahren) denotes the **loading of the operating system of a computer**.
After the boot process the computer is ready for operation (betriebsbereit).

**Bootstrapping** is the more general notion: a process in which a **simple system activates a
more complex system**.

## Context

Belongs to [[operating-system-basics]]. Booting is the bridge from bare hardware to a running
[[kernel]], carried out by the [[bios]].

## How it works

1. Power on; the [[bios]] firmware runs.
2. The BIOS checks whether all software and hardware components required for the start are ready
   for use.
3. The BIOS loads the operating system into RAM.
4. The OS takes over and starts the **initial process** — every further [[process]] is then
   created from it by [[fork]].

## Related

- [[bios]] — the firmware that performs the boot.
- [[process]] — the initial process created at the end of booting.
- [[fork]] — how all later processes descend from it.

## Common confusions

- **Booting vs. bootstrapping** — booting is the concrete OS-loading case; bootstrapping is the
  general "small system starts a bigger one" pattern the word comes from.
- The BIOS is not the OS; it only checks the hardware and loads the OS.

## Sources

- [[lecture-bs-1]] (p07)
