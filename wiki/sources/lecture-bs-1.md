---
title: "Lecture BS 1 — Exam Overview: OS Basics, Interrupts, Processes"
type: source
tags: [lecture, basics, interrupts, io, processes, ipc]
chapter: "01"
sources: [lecture-bs-1]
created: 2026-09-30
updated: 2026-09-30
---

# Lecture BS 1 — Exam Overview (Klausurübersicht)

- **Raw file:** `raw/lecture/bs_1.pdf`
- **Type:** lecture notes (handwritten summary, German) + exercise recap "BS-KVB-Ex1"
- **Ingested:** 2026-09-30
- **Pages:** 23 (scanned handwriting, no text layer — read via page rasterisation)

## Key takeaways

- An [[operating-system|operating system]] manages all [[resource|resources (Betriebsmittel)]],
  controls their allocation, and offers the user a [[virtual-machine|virtual machine]].
  Its three goals are convenience, efficiency and adaptability.
- The [[kernel]] is the lowest software layer with direct hardware access; [[device-driver|drivers]]
  hide device peculiarities behind device-independent interfaces (`open()`, `read()`, ...).
- An [[interrupt]] interrupts normal program execution; a [[trap]] is its synchronous,
  instruction-triggered counterpart. The [[interrupt-vector-table]] maps interrupt numbers to
  [[interrupt-service-routine|ISRs]], entered via a [[context-switch]] glue routine.
- Interrupts are handled either by **resumption** or **termination**; an interrupt *must* use
  resumption and *must never* use termination, a trap may use either.
- Three I/O techniques with rising CPU efficiency: programmed I/O → interrupt-driven I/O →
  [[direct-memory-access|DMA]].
- [[user-mode-and-kernel-mode|Two CPU modes]] separate applications from privileged operations;
  the transition happens only via a [[system-call]], which flips the mode bit through a trap.
- A [[process]] is a program in execution including registers, PC, stack and memory variables.
  It is represented in the OS by the [[process-control-block|PCB]] inside the [[process-image]].
- Process states: simple 2-state model → 5-state model → 7-state model with suspend states
  introduced by [[swapping]] — see [[process-state-model]].
- Processes are created with [[fork]] and overlaid with a new program via [[exec]]. The copy cost
  is avoided by [[copy-on-write]] (or historically `vfork()`).
- A dying child not collected by its parent becomes a [[zombie-process]]; a child outliving its
  parent becomes an [[orphan-process]] re-parented to PPID 1.
- [[interprocess-communication|IPC]] serves information exchange **and** synchronisation between
  cooperating processes; [[pipe|pipes]] (ring buffers) and [[signal|signals]] are the mechanisms.

## Detailed summary

### Part 1 — Foundations (p01–p08)

Pages 1–2 define the OS itself: software that manages all [[resource|Betriebsmittel]] of a
computing system, controls their allocation, and provides a [[virtual-machine]] abstraction over
the real hardware. Its tasks include running application programs, protecting concurrently
executed programs from each other, data exchange with devices, multi-user operation, error
detection and reporting, and memory management. The [[kernel]] contains user interfaces, process
management, memory management, I/O management, [[device-driver|device drivers]] and the system
call interface. The [[locality]] property motivates caching throughout the system.

Pages 3–5 cover the interrupt machinery: [[processor-registers]] (control/status vs.
user-visible), the definition of [[interrupt]] and [[trap]], the
[[interrupt-service-routine|ISR]], the [[interrupt-vector-table]], the two handling models
(resumption vs. termination), the three-column [[context-switch]] picture (interrupted program →
assembler glue routine → high-level handler), and [[interrupt-prioritisation]] by software polling
or a hardware prioritisation circuit with fixed or variable priorities. Nested ("gestufte")
interrupts are either processed sequentially (interrupts disabled inside the ISR) or
priority-controlled (only a higher-priority interrupt may preempt).

Page 6 compares the three I/O techniques; page 7 covers [[booting]], bootstrapping and the
[[bios|BIOS]]; page 8 covers [[system-call|system calls]] and the mode bit with its 4-step trap
flow.

### Part 2 — Processes (p09–p16)

Pages 9–12 define the [[process]], its attributes, the difference between process and program,
the roles of [[dispatcher]] and [[scheduler]], [[cpu-utilisation]], the 2-state, 5-state and
7-state [[process-state-model|process state models]] with [[swapping]], process termination
(voluntary vs. involuntary), and process creation via [[fork]].

Pages 13–16 add [[exec]] and the fork-exec mechanism, [[interprocess-communication|IPC]] with
[[pipe|pipes]] and [[signal|signals]], the OS management structures (primary process table →
[[process-image]], memory table, I/O table), the memory layout (program memory, globals/statics,
heap, stack), the [[process-control-block|PCB]] contents, and the 8-step [[process-switch]] —
explicitly contrasted with the plain [[context-switch]].

### Part 3 — Exercise recap "BS-KVB-Ex1" (p17–p23)

A written Q&A recap of exercise sheet 1 restating the process definition, PCB contents,
fork/exec-based process creation, zombies, dispatcher vs. scheduler and the purpose of IPC. It
then walks through a `fork()` program analysis (output order determined by `waitpid`, otherwise
non-deterministic; [[orphan-process]] when the parent exits first), the shell's copy overhead and
its remedies (`vfork()`, [[copy-on-write]]), shell redirection and piping (`>`, `<`, `|`), and a
short C API cheat sheet (`execlp`, `PATH`, `pipe`, `dup2`, `getppid`, `waitpid`/`WEXITSTATUS`).

## New pages created

Topics: [[operating-system-basics]], [[interrupts-and-io]], [[processes]],
[[interprocess-communication]]

Concepts: [[operating-system]], [[resource]], [[virtual-machine]], [[kernel]], [[locality]],
[[device-driver]], [[processor-registers]], [[interrupt]], [[trap]],
[[interrupt-service-routine]], [[interrupt-vector-table]], [[interrupt-prioritisation]],
[[context-switch]], [[direct-memory-access]], [[booting]], [[bios]],
[[user-mode-and-kernel-mode]], [[system-call]], [[process]], [[process-state-model]],
[[swapping]], [[process-control-block]], [[process-image]], [[process-switch]], [[dispatcher]],
[[scheduler]], [[cpu-utilisation]], [[fork]], [[exec]], [[copy-on-write]], [[zombie-process]],
[[orphan-process]], [[pipe]], [[signal]]

## Pages updated

[[overview]], [[index]] — first content in the wiki, nothing pre-existing to update.

## Contradictions / conflicts with existing wiki

None — this is the first ingested source.

Minor internal tension worth watching: p09 lists the process attributes as part of the *state*,
while p17 lists "program code, state, attributes" as three separate constituents. Recorded on
[[process]] under *Common confusions*.

## Open questions

- The `fork()` program analysed on p20 ("see Ex1") is not in the raw collection — the exact source
  code is unknown, only its behaviour was recorded. Adding `raw/exercise/` sheet 1 would let the
  worked example be traced properly.
- p22 and p23 reference "Praxis 1 & 2" practical tasks whose specifications are likewise not in
  `raw/`.
- The lecture does not say which scheduling strategy the dispatcher uses — presumably covered in a
  later chapter.
