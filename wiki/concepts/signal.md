---
title: Signal
type: concept
tags: [ipc, unix, interrupts]
chapter: "01"
sources: [lecture-bs-1, lecture-bs-2]
created: 2026-09-30
updated: 2026-09-30
---

# Signal

## Definition

A **signal** is a **system message to a running process**. It is a form of **asynchronous**
inter-process communication: **there is no instruction to indicate readiness to receive** (keine
Empfangsbereitschaft).

## Context

Belongs to [[interprocess-communication]]. It is the interrupt-like counterpart to the [[pipe]]:
no data, no rendezvous, just a notification.

## How it works

1. The process installs a **signal handling routine** (signal handler).
2. A signal is raised.
3. The process is **interrupted** and the handler is called.
4. The handler returns and normal execution resumes.

A process **can ignore signals**.

Which routine runs for which signal is recorded in the process's own **interrupt-vector table**
and can be changed with `sigaction` — see [[signal-handler]] for the concrete API, the `SIGUSR1`
example, the `sigemptyset`/`sigfillset` masking rules, and the effect of a signal on `sleep()`.

The structure deliberately mirrors the [[interrupt]] mechanism one level up: an asynchronous event
preempts the running code, a registered routine handles it, and execution resumes — the software
analogue of the [[interrupt-service-routine]].

## Related

- [[pipe]] — the data-carrying IPC mechanism.
- [[interrupt]] — the hardware analogue of the same pattern.
- [[process]] — signals are delivered to processes, not to programs.
- [[signal-handler]] — how the reaction is installed, masked and written.

## Common confusions

- **A signal carries no payload** — only the fact that it occurred.
- **There is no "receive" call.** Unlike a pipe read, the receiver does not ask for a signal; this
  "keine Empfangsbereitschaft" phrasing is what the exam wants.
- **Signals are not interrupts**, though the mechanism is analogous: signals are delivered by the
  OS to a process, interrupts by hardware to the CPU.
- The handler runs at an arbitrary point in the receiver's execution, which makes handler code
  hard to write safely.

## Sources

- [[lecture-bs-1]] (p14)
- [[lecture-bs-2]] (p04, p06–p07) — definition restated; handling covered in depth
