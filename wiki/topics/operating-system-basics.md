---
title: Operating System Basics
type: topic
tags: [basics, kernel, hardware, modes]
chapter: "01"
sources: [lecture-bs-1]
created: 2026-09-30
updated: 2026-09-30
---

# Operating System Basics

## Summary

An [[operating-system|operating system]] (Betriebssystem, BS) is the software layer between the
hardware and the application programs. It manages all [[resource|resources (Betriebsmittel)]] of a
computing system, controls and accounts for their allocation, and presents the user with a
[[virtual-machine]] — a simplified, uniform interface to messy real hardware. Its central
component is the [[kernel]], the lowest software layer with direct hardware access. Because
applications must not be allowed to touch hardware directly, the CPU separates
[[user-mode-and-kernel-mode|user mode from kernel mode]], and the only legitimate door between
them is the [[system-call]]. Before any of this can run, the machine must be brought up by
[[booting]] via the [[bios]].

## Why it exists

Raw hardware is inconvenient (every device has its own registers and quirks), inefficient when
shared naively (who gets the CPU, the disk, the memory?), and unsafe (one buggy program could
destroy another). The OS exists to provide **convenience** (Bequemlichkeit), **efficiency**
(Effizienz) and **adaptability** (Anpassbarkeit) — the three goals named in the lecture.

Layering, as sketched in the notes:

```
User
 ↕
Anwendung (application)
 ↕
BS (operating system)
 ↕
HW (hardware)
```

## Key concepts

- [[operating-system]] — the definition itself (manage, allocate, virtualise)
- [[resource]] — all allocatable hardware & software components
- [[virtual-machine]] — the abstraction offered to users and programs
- [[kernel]] — lowest software layer, direct hardware access
- [[device-driver]] — hides device peculiarities; device-dependent vs. device-independent
  interfaces
- [[locality]] — temporal and spatial locality, the property that makes caching work; chapter 4
  turns it into the **Lokalitätsprinzip** and makes it examinable
- [[memory-hierarchy]] — registers → cache → main memory → disk, the structure locality justifies
  (developed fully in [[memory-management]])
- [[processor-registers]] — control/status vs. user-visible registers
- [[user-mode-and-kernel-mode]] — privilege separation and the mode bit
- [[system-call]] — the controlled transition into the kernel
- [[booting]] and [[bios]] — how the system comes up

## Mechanisms / algorithms

**Tasks of the OS (Aufgaben des Betriebssystems)**, per the lecture:

1. Execution of application programs
2. Protection of concurrently executed programs from one another
3. Data exchange with devices
4. Multi-user operation (Mehrbenutzerbetrieb)
5. Error detection and reporting
6. Memory management (Speicherverwaltung)

**Contents of the [[kernel]]:** user interfaces, process management, memory management, I/O
management, [[device-driver|device drivers]], system-call interface.

## Trade-offs

| Dimension | Device-dependent interface | Device-independent interface |
|---|---|---|
| Device features | All properties fully exploitable | Only the common denominator |
| Cost | I/O system must be extended per driver | Uniform `open()`, `read()`, ... |
| Extensibility | Poor | Easy, independent of existing applications |
| Virtual devices | Not supported | Supported |

The general pattern: **abstraction buys convenience and portability at the price of losing
device-specific capability and some performance.** The same trade-off reappears in
[[virtual-machine]] and in the [[system-call]] boundary (safety costs a mode switch).

## Exam relevance

The lecture's exam overview leads with these definitions, so they are near-certain question
material. Expect to reproduce verbatim-style definitions:

- *"Definieren Sie Betriebssystem."* — the three-part definition (manages Betriebsmittel /
  controls allocation / offers a virtual machine).
- *"Nennen Sie die drei Ziele eines Betriebssystems."* — Bequemlichkeit, Effizienz, Anpassbarkeit.
- *"Was ist ein Betriebsmittel?"*
- *"Nennen Sie Aufgaben des Betriebssystems."* — list 4–6 of the six above.
- *"Vergleichen Sie geräteabhängige und geräteunabhängige Schnittstellen."* — the table above.
- *"Was ist die Lokalitätseigenschaft?"* — and the distinction temporal/spatial.
- *"Wodurch unterscheiden sich Benutzer- und Kernmodus?"* — see
  [[user-mode-and-kernel-mode]].

## Open questions

- The notes list six OS tasks but do not rank them; TUHH exams sometimes ask for "die drei
  wichtigsten" — unclear which three the lecture would accept.

## Sources

- [[lecture-bs-1]] (p01–p08)
