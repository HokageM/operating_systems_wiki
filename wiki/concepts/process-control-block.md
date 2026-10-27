---
title: Process Control Block (PCB)
type: concept
tags: [process, pcb, data-structures]
chapter: "01"
sources: [lecture-bs-1, lecture-bs-2, lecture-bs-5]
created: 2026-09-30
updated: 2026-09-30
---

# Process Control Block (PCB)

## Definition

The **process control block** (Prozesskontrollblock, PCB) **gathers all information belonging to
one single [[process]]**. It is the data structure by which a process is represented in the
operating system.

## Context

Belongs to [[processes]]. The PCB is one part of the [[process-image]]; the OS's **primary process
table** has one entry per process pointing at it.

## How it works

**Contents of the PCB:**

1. **Process identification** (Prozesserkennung): PID, PPID, UID
2. Contents of the **user-visible registers** (see [[processor-registers]])
3. Contents of the **control and status registers** (e.g. the program counter)
4. **Address in memory** (Adresse im Speicher)
5. **References to the stacks** (Verweise auf Stacks)
6. **Scheduling and state information** (Scheduling- & Zustandsinformationen)

During a [[process-switch]] the PCB of the outgoing process is updated (state saved) and the PCB
of the incoming process is read to load its state — steps 3 and 6 of the eight.

## Related

- [[process-image]] — the PCB plus user data, program and stacks.
- [[process-switch]] — reads and writes PCBs.
- [[process-state-model]] — the state stored in field 6.
- [[processor-registers]] — fields 2 and 3.
- [[thread-control-block]] — the per-thread counterpart, holding only PC, registers, stack pointer
  and the thread-local memory pointer.
- [[scheduling]] — the scheduling and state information in the PCB is the scheduler's input.
- [[file-metadata]] — the same idea for files; the notes even call an [[i-node]] a *file control block*.

## Common confusions

- **PCB ≠ process image.** The PCB is the *control record*; the image is the whole in-memory
  representation including code and data. See [[process-image]].
- **The PCB does not contain the program** — only the address of it.
- **PCB ≠ TCB.** A [[thread-switch]] exchanges only the [[thread-control-block]]; the PCB and the
  memory tables stay put. Knowing which of the two is exchanged is the crux of the chapter-2 exam
  question.
- *"Wodurch wird ein Prozess im BS repräsentiert?"* → "durch den PCB". Then reproduce the six
  bullets; the exercise recap asks exactly this.

## Sources

- [[lecture-bs-1]] (p15, p17)
- [[lecture-bs-2]] (p05) — PCB vs. TCB on a switch
- [[lecture-bs-5]] (p01, exercise S-1 c) — the PCB is updated in steps 3 and 6 of a process switch.
