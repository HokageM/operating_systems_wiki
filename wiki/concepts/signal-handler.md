---
title: Signal Handler
type: concept
tags: [ipc, signals, unix, c]
chapter: "02"
sources: [lecture-bs-2]
created: 2026-09-30
updated: 2026-09-30
---

# Signal Handler

## Definition

A **signal handling routine** (Signalbehandlungsroutine) is the function a [[process]] registers
to be executed when a given [[signal]] arrives. Which routine runs for which signal is recorded in
the process's **interrupt-vector table (Unterbrechungsvektor-Tabelle)**, and that behaviour **can
be changed with `sigaction`**.

## Context

Belongs to [[interprocess-communication]]. It is the software mirror of the
[[interrupt-service-routine]], and the table it lives in is the same structure that [[thread|
threads]] share with each other ([[thread-control-block]]).

## How it works

### Raising a signal

`SIGUSR1` is the user-defined signal used in the exercise. When it arrives, the handler prints
`received signal: SIGUSR1`. It can be raised:

- from a Linux shell: `kill -USR1 <pid>` — with the PID of the running process;
- from inside a program: `kill(pid, SIGUSR1)`.

### Signals arriving *during* the handler — the signal mask

What happens if another signal arrives while the handler is still running is controlled by
`new_action.sa_mask`:

| Mask setup | Effect |
|---|---|
| `sigemptyset(&new_action.sa_mask)` | every signal **except** `SIGUSR1` is allowed inside the handler → all other signals **interrupt** the currently running handler |
| `sigfillset(&new_action.sa_mask)` | **every** signal is blocked during the handler and is only delivered **after** the handling has finished |

So `sigfillset` serialises signal handling, `sigemptyset` permits nesting — exactly the same
choice as sequential vs. priority-controlled nested [[interrupt-prioritisation|interrupts]] one
layer down.

### Signals and `sleep()`

A signal **does** affect `sleep()` — `SIGALARM` is the example. `sleep()` returns the *remaining*
time when it is cut short, so a reliable 5-second period must restart it with the remainder:

```c
while (true) {
    /* ... print "active" ... */
    int seconds = 5;
    while ((seconds = sleep(seconds)) != 0);
}
```

The inner loop is the whole point: without it, any signal shortens the interval.

## Related

- [[signal]] — the event being handled.
- [[interrupt-service-routine]] — the hardware-level analogue.
- [[interrupt-prioritisation]] — the same nesting-vs-serialising trade-off.
- [[interrupt-vector-table]] — the same name is used for the per-process signal table.

## Common confusions

- **`sigemptyset` does not mean "no protection at all"** — the signal currently being handled is
  still blocked; only the *other* signals get through.
- **`sleep()` does not sleep the full time if interrupted.** Assuming it does is the trap the
  exercise is built around.
- A handler runs at an arbitrary point in the receiver's execution, so handler code must be
  re-entrancy-safe.

## Sources

- [[lecture-bs-2]] (p06, p07)
