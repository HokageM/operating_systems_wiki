---
title: Inter-Process Communication
type: topic
tags: [ipc, pipes, signals, synchronisation]
chapter: "01"
sources: [lecture-bs-1, lecture-bs-2]
created: 2026-09-30
updated: 2026-09-30
---

# Inter-Process Communication

## Summary

Because every [[process]] has its own private address space, two processes cannot simply share a
variable. Inter-process communication (Interprozesskommunikation, IPC) is the set of OS mechanisms
that let cooperating processes nevertheless work together. The lecture names two purposes:
**exchange of information** between cooperating processes and **synchronisation** of cooperating
processes. Lecture 1 covers two mechanisms — the [[pipe]], a ring buffer carrying a byte stream
between two processes, and the [[signal]], an asynchronous system message that interrupts the
receiving process and runs a handler.

## Why it exists

Address-space isolation is what makes processes safe, but isolation without any channel would make
them useless for cooperation. IPC re-opens a *controlled* channel: the OS mediates it, so the
isolation guarantee survives.

## Key concepts

- [[pipe]] — ring buffer, unidirectional byte stream, created with `pipe()` and wired with `dup2()`
- [[signal]] — asynchronous notification, handled by a signal handler or ignored
- [[signal-handler]] — `sigaction`, signal masks, and what happens when signals nest
- [[process]] — the participants
- [[fork]] — how the two communicating processes usually come to exist (they must be related to
  share an unnamed pipe)
- [[thread]] — the alternative that makes IPC unnecessary: threads share one address space and
  communicate through plain variables, **without the OS**

Shared memory and shared variables need [[mutual-exclusion]] — see
[[concurrency-and-synchronisation]]. A [[semaphore]] is itself an IPC mechanism: unlike a
[[mutex]] it may be released by a *different* process, which makes it usable for cross-process
signalling. The [[pipe]] is the OS's own [[producer-consumer]] buffer.

## Mechanisms / algorithms

| | [[pipe]] | [[signal]] |
|---|---|---|
| Carries | A byte stream | A bare notification (a signal number) |
| Direction | Unidirectional (write end → read end) | Sender → receiver |
| Synchrony | Receiver blocks on `read()` — readiness to receive is explicit | Asynchronous — **no** indication of readiness to receive exists |
| Buffering | Ring buffer (Ringpuffer) in the kernel | None |
| Receiver control | Chooses when to read | Is interrupted; may install a handler or ignore the signal |
| Typical use | Streaming data, shell pipelines `ls \| wc -l` | Termination requests, child-exit notification, timers |

### Pipe setup after fork

The parent calls [[fork]]; parent and child inherit both pipe file descriptors. Each side then
closes the end it does not use and redirects the other onto a standard stream:

```c
int pipefd[2];
pipe(pipefd);
// child (writer):
dup2(pipefd[1], STDOUT_FILENO);
// parent (reader):
dup2(pipefd[0], STDIN_FILENO);
```

A shell pipeline `ls | wc -l` is exactly this: the output of `ls` is piped to `wc -l` as its input.
Redirection to and from files uses the same `dup2()` trick: `ls > output.txt`,
`wc -l < output.txt`.

### Signal flow

The process installs a signal handling routine → a signal is raised → the process is interrupted →
the handler runs → the handler returns → normal execution resumes. A process may also ignore
signals.

## Trade-offs

- **Pipes** are simple and self-synchronising (a reader blocks until data arrives) but only connect
  *related* processes (unnamed pipes) and only in one direction — two pipes are needed for a
  dialogue.
- **Signals** are cheap and need no prior agreement, but carry almost no information and arrive at
  an arbitrary point in the receiver's execution, which makes handlers hard to write correctly.
- Both put the OS in the middle: safety and mediation cost a [[system-call]] per operation
  compared to shared memory.
- **IPC is the price of isolation.** Chapter 2 makes this explicit: because only the *code* can be
  shared between processes, they need "pipes, shared memory or similarly **expensive mechanisms**"
  — whereas [[threads|threads]] of one process share the data segment and need none of it. Choose
  processes when you want protection, threads when you want cheap communication.

## Exam relevance

- *"Wozu dient die Interprozesskommunikation?"* — asked verbatim in the exercise recap; the
  expected answer is the two bullets: Austausch von Informationen **und** Synchronisation
  kooperierender Prozesse. Naming only the first costs points.
- *"Was ist eine Pipe?"* — "ein Ringpuffer, der die Kommunikation zweier Prozesse unterstützt".
- *"Was ist ein Signal?"* — asynchronous system message to a running process, no
  Empfangsbereitschaft, handler or ignore.
- *"Erklären Sie `ls | wc -l` / `ls > output.txt` / `wc -l < output.txt`."* — a practical exercise
  question from p22.
- Writing the `pipe()` + `dup2()` wiring for a child→parent channel (p23).

## Open questions

- Named pipes (FIFOs), shared memory and message queues are not mentioned in this source; unclear
  whether the module covers them at all.
- Synchronisation primitives (semaphores, mutexes) are named as a *purpose* of IPC here but no
  mechanism is given — presumably a later chapter.

## Sources

- [[lecture-bs-1]] (p13–p14, p19, p22–p23)
- [[lecture-bs-2]] (p01, p04, p06–p07) — IPC as the expensive alternative to threads; signal
  handling in depth
