---
title: User Mode and Kernel Mode
type: concept
tags: [basics, protection, modes]
chapter: "01"
sources: [lecture-bs-1]
created: 2026-09-30
updated: 2026-09-30
---

# User Mode and Kernel Mode

## Definition

The CPU runs in one of two privilege states. **Kernel mode** (Systemmodus, Kernmodus) permits all
operations; **user mode** (Benutzermodus) permits only a restricted subset. Applications are
executed in user mode; only in the [[kernel]] may hardware be accessed, new processes created,
system-relevant data structures manipulated and all registers accessed.

Which mode is active is determined by the **mode bit**, and **the mode bit can only be changed by
a [[system-call]]**.

## Context

Belongs to [[operating-system-basics]]. This is the hardware support that makes clause 2 of the
[[operating-system]] definition — *controlled* allocation — enforceable rather than merely polite.

## How it works

| | **User mode** | **Kernel mode** |
|---|---|---|
| Machine instructions | Only selected ones | All |
| Hardware access | Only via the OS | Full |
| System data | No access, or read-only | Full access |

Transition: the application executes a [[trap]] (a [[system-call]]); the CPU enters the handler,
the mode bit is set to 1, the OS performs the service, and on return the mode bit is reset to 0.
This transition is a [[context-switch]], **not** a [[process-switch]].

## Related

- [[system-call]] — the only door between the modes.
- [[trap]] — the instruction-level mechanism behind that door.
- [[kernel]] — the code that runs in kernel mode.
- [[processor-registers]] — the mode bit is a control/status register.

## Common confusions

- **The mode bit is not settable by the program directly** — if it were, the protection would be
  worthless. Only a system call flips it.
- **Entering kernel mode does not switch processes.** The same process continues, now executing OS
  code — see [[context-switch]].
- "Kernel mode" is a CPU state; the [[kernel]] is the software. See [[kernel]].

## Sources

- [[lecture-bs-1]] (p07, p08)
