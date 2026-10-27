---
title: Processes
type: topic
tags: [process, scheduling, pcb, states, fork]
chapter: "01"
sources: [lecture-bs-1, lecture-bs-5]
created: 2026-09-30
updated: 2026-09-30
---

# Processes

## Summary

A [[process]] is a program in execution, including the current values of the registers, the
program counter, the stack and the memory variables. Each process owns a private address space and
conceptually its own virtual processor, which produces pseudo-parallel execution on a single CPU.
The OS represents each process by a [[process-control-block]] inside its [[process-image]], and
tracks its life cycle with a [[process-state-model]] — 2, 5 or 7 states depending on how much
detail (and [[swapping]]) is modelled. The [[scheduler]] decides *when* and *to whom* to switch,
the [[dispatcher]] performs the switch, and the switch itself is the 8-step [[process-switch]].
Processes are born from [[fork]] and change their program via [[exec]].

## Why it exists

A program is a static description of an algorithm — a file. To run several programs at once, to
interleave them on one CPU, to protect them from each other and to account for their resource
usage, the OS needs a *runtime* object with identity, state and ownership. That object is the
process. Without it, there is nothing to schedule, nothing to suspend and resume, and no unit to
which memory and open devices can be attributed.

## Key concepts

- [[process]] — the definition, attributes, and the process/program distinction
- [[process-control-block]] — the OS's record of a process
- [[process-image]] — the total representation of a process in memory
- [[process-state-model]] — 2-state, 5-state and 7-state models
- [[swapping]] — what introduces the suspend states
- [[process-switch]] — the 8 steps
- [[context-switch]] — the cheaper thing a process switch is often confused with
- [[scheduler]] and [[dispatcher]] — decision vs. execution
- [[cpu-utilisation]] — the metric
- [[fork]], [[exec]], [[copy-on-write]] — creation and program replacement
- [[zombie-process]], [[orphan-process]] — the two death anomalies

Chapter 3 adds what happens when several processes contend for the same
[[resource]]: see [[concurrency-and-synchronisation]] for [[mutual-exclusion]] and
[[deadlocks]] for what goes wrong when they block on each other.
- [[interprocess-communication]] — how processes talk once they exist
- [[threads]] — the cheaper unit of execution *inside* a process, introduced because process
  creation and process switching are expensive

## Mechanisms / algorithms

### Process attributes (Prozessattribute)

Process ID (PID), execution state (Ablaufzustand), priority, memory references
(Speicherverweise), results, I/O status information, accounting/management information
(Verwaltungsinformationen).

### Process creation (Prozesserzeugung)

1. At system boot an **initial process** is started ([[booting]]).
2. Every further process is created by an existing process via the [[fork]] system call.
3. The child typically calls [[exec]] to replace its program image — the **fork-exec mechanism**.

### Process termination (Prozessterminierung)

| Voluntary (freiwillig) | Involuntary (unfreiwillig) |
|---|---|
| Normal end of the program | Time limit exceeded |
| Call of `exit()` | Not enough memory available |
| | Arithmetic error |
| | I/O error |

Abnormal outcomes: [[zombie-process]] (child ends, parent has not collected it) and
[[orphan-process]] (parent ends first, child is re-parented to PPID 1).

### OS management structures (Verwaltungsstrukturen)

A **primary process table** holds one entry per process (P1, P2, ...); each entry points to that
process's [[process-image]]. Alongside it the OS keeps a **memory table** (Speichertabelle) and an
**I/O table**.

### Memory layout of a process

Program memory (Programmspeicher) · memory for global and static local variables · heap · stack.

## Trade-offs

- **More states = more accurate, harder to reason about.** The 2-state model is enough to explain
  dispatching; the 5-state model is needed to explain blocking; the 7-state model is only needed
  once memory is scarce and [[swapping]] exists.
- **fork's copy semantics**: a full copy is clean but expensive, especially when the child
  immediately calls [[exec]] and throws the copy away. [[copy-on-write]] resolves this, at the
  cost of page-fault machinery. `vfork()` resolves it by restricting the child instead.
- **Process switch cost**: isolation between processes is paid for on every switch (PCB save/load,
  MMU reprogramming) — which is exactly why a plain [[context-switch]] into the kernel is kept
  distinct and cheap.
- **Execution speed is not reproducible**: the programmer gains concurrency but must not make any
  assumption about the temporal course of execution.
- **Isolation vs. cost, resolved in chapter 2.** Everything that makes a process safe — its own
  address space, its own file table, its own memory-management entries — is what makes creating
  and switching it expensive. [[threads|Threads]] keep the concurrency and drop the isolation.

## Exam relevance

This is the densest exam block of lecture 1 — the exercise recap (p17–p23) asks almost exclusively
about processes.

- *"Was ist ein Prozess?"* / *"Woraus besteht ein Prozess?"* — program code, state, attributes.
- *"Wodurch wird ein Prozess im BS repräsentiert?"* — the [[process-control-block]] and its six
  content bullets. Reproduce the list.
- *"Unterschied Prozess und Programm?"* — static description vs. execution; one program, many
  processes; non-uniform execution speed.
- *"Zeichnen Sie das Prozesszustandsmodell."* — the 5-state and 7-state diagrams; label the
  transitions (Admit, Dispatch, Timeout, Event Wait, Event Occurs, Release, Suspend, Activate).
- *"Dispatcher vs. Scheduler?"* — asked verbatim in the exercise recap.
- *"Wie werden Prozesse erzeugt?"* — initial process at boot, then `fork()` + `exec()`.
- *"Was passiert bei fork()?"* — own address space, inherits data/code/PC, gets its own PID; and
  the return values 0 / >0 / −1.
- *"Wie entsteht ein Zombie- / Waisenprozess?"*
- *"Nennen Sie die Schritte eines Prozesswechsels."* — the 8 steps of [[process-switch]].
- *"Wann findet KEIN Prozesswechsel statt?"* — system call, switch to kernel mode, interrupt.
- Calculating [[cpu-utilisation]] from a small timeline.
- *"Präemptiv vs. nicht-präemptiv?"* and the scheduling strategies — see [[scheduling]], added by
  chapter 05.

## Open questions

- p09 counts the attributes as part of the process *state*, while p17 lists "program code, state,
  attributes" as three separate constituents. Noted on [[process]]; harmless for exam answers but
  worth confirming against the lecture slides.
- Scheduling *strategies* (which process the scheduler picks) are **resolved in chapter 05** — see
  [[scheduling]]. [[lecture-bs-5]] covers [[first-come-first-served]], [[shortest-process-next]],
  [[priority-scheduling]] and [[round-robin]], organised by
  [[preemptive-and-non-preemptive-scheduling]]. They are examinable, with a full Gantt-chart
  exercise.
- A process's memory is developed much further in chapter 4: what [[process-image]] calls "the
  representation in memory" is cut into [[page-and-frame|pages]] by [[paging]] and need not even
  be resident — see [[virtual-memory]].

## Sources

- [[lecture-bs-1]] (p09–p23)
- [[lecture-bs-5]] (p01–p09, exercises S-1 and S-2) — the scheduling of these processes.
