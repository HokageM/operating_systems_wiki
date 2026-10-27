---
title: Process
type: concept
tags: [process, definition]
chapter: "01"
sources: [lecture-bs-1, lecture-bs-2]
created: 2026-09-30
updated: 2026-09-30
---

# Process

## Definition

A **process** (Prozess) is **a program in execution** (ein in Ausführung befindliches Programm),
**including the current values of the registers, the program counter, the stack and the memory
variables**.

A process consists of **program code**, **state** (Zustand) and **attributes** (Attribute).

## Context

The central object of [[processes]]. It is the unit to which the OS allocates
[[resource|resources]], and the thing the [[scheduler]] schedules.

## How it works

A process owns:

- a **private address space** — the part of main memory it may use;
- conceptually **its own virtual processor**, which produces *pseudo-parallel* process execution
  on a single CPU. From the process's own perspective it is always executing.

**Process attributes (Prozessattribute):** process ID (PID), execution state (Ablaufzustand),
priority, memory references (Speicherverweise), results, I/O status information, accounting and
management information (Verwaltungsinformationen).

The OS represents the process by a [[process-control-block]], which lives inside the
[[process-image]] and is reached via the primary process table. Its life cycle is described by the
[[process-state-model]]; it is created with [[fork]] and destroyed voluntarily (`exit()`, normal
end) or involuntarily (time limit exceeded, not enough memory, arithmetic error, I/O error).

## Related

- [[process-control-block]] — how the OS stores it.
- [[process-image]] — its total representation in memory.
- [[process-state-model]] — its life cycle.
- [[fork]], [[exec]] — birth and program replacement.
- [[interprocess-communication]] — how it talks to others, since address spaces are private.
- [[thread]] — the strand of execution *inside* a process; a process always has at least one (its
  main thread), and all of them share its code and data segment.
- [[deadlock]] — the definition is about a *set of processes*, each waiting for an event only
  another member can cause.
- [[deadlock-recovery]] — aborting a process is the crudest way out of a deadlock.

## Common confusions

- **Process vs. program.** A *program* is the static description of an algorithm; one program can
  be executed by several processes at the same time. A *process* is a program in execution.
- **Execution speed is not uniform and not reproducible.** The programmer must not make
  assumptions about the temporal course of execution — this is stated twice in the source and is a
  favourite exam sentence.
- **Pseudo-parallel ≠ parallel.** On one CPU only one process really runs at a time.
- **Process vs. thread.** The process owns the resources (address space, file table); the
  [[thread]] owns only a program counter, registers and a stack. The process is what is
  *isolated*; the thread is what is *executed*. Chapter 2 adds that creating and switching
  processes is expensive precisely because of everything in the list above — see
  [[thread-switch]].

> [!question] Unverified — internal tension in the notes
> p09 presents the attributes as part of the process *state* ("Zustand = die Attribute, die ihn
> halten"), while p17 lists "Programm-Code, Zustand, Attribute" as three separate constituents.
> The three-part list is the safer exam answer.

## Sources

- [[lecture-bs-1]] (p09, p10, p17)
- [[lecture-bs-2]] (p01) — process/thread contrast
