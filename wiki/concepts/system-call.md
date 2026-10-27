---
title: System Call
type: concept
tags: [basics, kernel, traps]
chapter: "01"
sources: [lecture-bs-1]
created: 2026-09-30
updated: 2026-09-30
---

# System Call

## Definition

A **system call** (Systemaufruf) is **a method to execute all functionalities provided by the
operating system**. In doing so, **control of the program is handed over to the [[kernel]]**.

It serves as the interface to application programs and as the point at which access rights to
files and devices are monitored.

## Context

Belongs to [[operating-system-basics]]. It is the only legitimate transition from user mode to
kernel mode — see [[user-mode-and-kernel-mode]] — and it is implemented as a [[trap]].

## How it works

The 4-step flow from the notes:

1. The user program deposits the **parameters** and the **ID of the function** in main memory and
   **generates a trap**.
2. The processor interrupts the application and starts the **trap handler**.
3. CPU control is delegated to the OS; the **mode bit is set to 1** (kernel mode).
4. The CPU works off the call. At the end the invoked function copies the result into the memory
   area of the application, the trap is closed, the process returns, and the **mode bit is reset
   to 0**.

```
User process (Benutzermodus)
      │  System Call
      ▼
OS kernel executes the system service (Systemmodus)
      │  Rückkehr vom System Call
      ▼
User process continues
```

## Related

- [[trap]] — the mechanism; a system call is a deliberate trap.
- [[user-mode-and-kernel-mode]] — what it switches between.
- [[context-switch]] — what actually happens (not a [[process-switch]]).
- [[fork]], [[exec]] — concrete system calls covered in this chapter.

## Common confusions

- **A system call is not a normal function call** — it crosses a privilege boundary via a trap.
- **No [[process-switch]] takes place.** The calling process continues, in kernel mode. Explicitly
  asked in the notes: "Wann findet kein Prozesswechsel statt?" → system call, switch to kernel
  mode, interrupt.
- The parameters are passed *in memory* plus a function ID, not "as arguments" in the C sense.

## Sources

- [[lecture-bs-1]] (p08, p16)
