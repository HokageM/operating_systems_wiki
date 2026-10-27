---
title: Kernel
type: concept
tags: [basics, kernel]
chapter: "01"
sources: [lecture-bs-1]
created: 2026-09-30
updated: 2026-09-30
---

# Kernel

## Definition

The **kernel** (BS-Kern) is the central component of an [[operating-system]]: the **lowest
software layer, with direct access to the hardware**.

## Context

Belongs to [[operating-system-basics]]. The kernel is what runs in
[[user-mode-and-kernel-mode|kernel mode]], and entering it is what a [[system-call]] does.

## How it works

Contents of the kernel, per the lecture:

- user interfaces
- process management (see [[process]], [[scheduler]], [[dispatcher]])
- memory management (Speicherverwaltung)
- I/O management (see [[interrupts-and-io]])
- [[device-driver|device drivers]]
- the [[system-call]] interface

Only inside the kernel may all operations be carried out: hardware access, creation of new
processes, manipulation of system-relevant data structures, and access to all registers.

## Related

- [[operating-system]] — the kernel is its core, not the whole of it.
- [[user-mode-and-kernel-mode]] — the privilege level the kernel runs at.
- [[system-call]] — the only legitimate entry point from an application.
- [[device-driver]] — kernel components with access to device registers.

## Common confusions

- **Kernel vs. operating system** — see [[operating-system]]; shells, editors and utilities are
  part of an OS distribution but not of the kernel.
- **Kernel mode vs. kernel** — the mode is a CPU privilege state; the kernel is the code that runs
  in it. An interrupt handler entering kernel mode is a [[context-switch]], not a
  [[process-switch]].

## Sources

- [[lecture-bs-1]] (p02, p07)
