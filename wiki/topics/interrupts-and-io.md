---
title: Interrupts and I/O
type: topic
tags: [interrupts, io, dma, hardware]
chapter: "01"
sources: [lecture-bs-1, lecture-bs-5]
created: 2026-09-30
updated: 2026-09-30
---

# Interrupts and I/O

## Summary

An [[interrupt]] is a signal that interrupts normal program execution so that a different piece of
processing can be carried out. Hardware devices raise interrupts asynchronously; instructions
raise [[trap|traps]] synchronously. On arrival, the CPU looks the interrupt number up in the
[[interrupt-vector-table]] and jumps to the associated [[interrupt-service-routine]], saving and
restoring the interrupted program's registers around it via a [[context-switch]]. When several
devices can interrupt at once the system needs [[interrupt-prioritisation]]. The whole apparatus
exists mainly to serve **I/O**: it lets the CPU stop polling devices, which is the step from
programmed I/O to interrupt-driven I/O and finally to [[direct-memory-access|DMA]].

## Why it exists

Without interrupts the processor must busy-wait on every slow device (programmed I/O), wasting
essentially all of its cycles. Interrupts improve the instruction throughput of processors by
letting the CPU do useful work and be *notified* when the device is ready. DMA takes the idea one
step further: the CPU is not even involved per word, only per block.

## Key concepts

- [[interrupt]] — asynchronous, hardware-triggered, not reproducible, unpredictable
- [[trap]] — synchronous, instruction-triggered, predictable and reproducible
- [[interrupt-service-routine]] — the handler that runs
- [[interrupt-vector-table]] — number → ISR start address
- [[context-switch]] — save/restore around the handler (glue routine)
- [[interrupt-prioritisation]] — software polling vs. hardware prioritisation circuit
- [[direct-memory-access]] — block transfer without per-word CPU involvement
- [[processor-registers]] — what has to be saved and restored
- [[system-call]] — the most important trap in practice

## Mechanisms / algorithms

### Handling models

| Model | Meaning | Trap | Interrupt |
|---|---|---|---|
| **Resumption** (Wiederaufnahme) | Successful handling of the exception leads to continuation of the interrupted process | *may* | **must** |
| **Termination** (Beendigung) | The exception cannot be handled; a severe error is signalled and the interrupted process is aborted | *may* | **must never** |

This asymmetry is the memorable part: an asynchronous interrupt has nothing to do with the running
process, so killing that process would be arbitrary — it must always be resumed.

### The three I/O techniques

| Technique | Who moves the data | CPU during transfer | Notification | Drawback |
|---|---|---|---|---|
| **Programmed I/O** | I/O module performs the action, CPU polls a status register | Busy-polling | None — CPU polls | Time-consuming; CPU needlessly occupied |
| **Interrupt-driven I/O** | CPU, word by word | Free between interrupts | [[interrupt]] | Still processor-intensive: every word passes through the CPU |
| **[[direct-memory-access|DMA]]** | DMA controller, directly to/from memory | Free for the whole block | [[interrupt]] at end of block | Extra hardware; bus contention |

DMA flow as sketched in the notes: CPU issues a "read block" command to the I/O module → CPU does
something else → DMA module raises an interrupt when done → CPU reads the status of the DMA module.
The CPU passes the DMA module: the type of operation, the address of the I/O device, and the
number of words to transfer.

### Nested / staged interrupts (gestufte Interrupts)

- **Sequential processing** — interrupt processing is disabled inside the ISR, so interrupts are
  handled strictly one after another.
- **Priority-controlled processing** — an ISR may only be interrupted by an interrupt of *higher*
  priority.

ISRs should be as short as possible, so that the window in which interrupts are suppressed/masked
stays small.

## Trade-offs

- **Short ISR vs. complete handling**: the less you do in the ISR, the fewer interrupts you lose —
  hence the split between the assembler glue routine and the real handler.
- **Sequential vs. priority-controlled nesting**: sequential is simple and safe but lets urgent
  events wait; priority-controlled is responsive but needs re-entrant handlers and a priority
  scheme.
- **Software vs. hardware prioritisation**: polling needs no extra hardware but costs CPU time and
  fixes the order; a prioritisation circuit is fast and supports variable priorities but is extra
  silicon. See [[interrupt-prioritisation]].
- **Interrupt-driven I/O vs. DMA**: DMA wins for large blocks, but for a single byte the setup
  overhead is not worth it.

## Exam relevance

- *"Was ist ein Interrupt?"* and *"Worin unterscheiden sich Interrupt und Trap?"* — the
  reproducible/predictable axis is the expected answer ([[trap]]).
- *"Erklären Sie Wiederaufnahme und Beendigung."* — including the may/must/must-never table above;
  this is a favourite because the asymmetry is easy to get wrong.
- *"Nennen und vergleichen Sie die drei E/A-Techniken."* — the comparison table; the drawback
  column is what earns the points.
- *"Was macht ein DMA-Baustein und welche Informationen bekommt er?"* — see
  [[direct-memory-access]].
- *"Wie werden Interrupts priorisiert?"* — software polling vs. hardware circuit, fixed vs.
  variable priorities.
- *"Warum sollen ISRs kurz sein?"*
- Drawing the three-column [[context-switch]] diagram is a plausible sketch question.

## Open questions

- The notes do not state how the interrupt *number* is delivered to the CPU (vectored interrupt
  vs. polling the controller) — only that it indexes the [[interrupt-vector-table]].

## Sources

- [[lecture-bs-1]] (p03–p06)
- [[lecture-bs-5]] (p02, p06, p13) — the [[clock-interrupt]] as the OS’s own periodic interrupt source, and [[asynchronous-file-access]] as non-blocking I/O.
