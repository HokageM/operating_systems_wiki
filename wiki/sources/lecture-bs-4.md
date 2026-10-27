---
title: Lecture BS 4 — Memory Management (Speicherverwaltung)
type: source
tags: [lecture, memory, paging, virtual-memory]
chapter: "04"
sources: [lecture-bs-4]
created: 2026-09-30
updated: 2026-09-30
---

# Lecture BS 4 — Memory Management (Speicherverwaltung)

- **Raw file:** `raw/lecture/bs_4.pdf`
- **Type:** lecture notes + exercise recap ("BS-KVB-Ex 4") + two bonus sheets ("Zusatz 1", "Zusatz
  2") + a practical section ("Praxis 4")
- **Length:** 29 pages, handwritten German, scanned without a text layer (read page by page as
  images)
- **Chapter heading in the notes:** **BS-KVB**
- **Ingested:** 2026-09-30

## Key takeaways

- The chapter is organised as a **numbered progression of allocation variants**: 1
  [[static-partitioning]] → 2 [[dynamic-partitioning]] → 2a [[paging]] → 2b [[virtual-memory]].
  Each variant fixes the previous one's defect and pays a new price. Knowing this chain is knowing
  the chapter.
- [[memory-management]] is defined by **four tasks**: manage free/occupied areas, assign at
  runtime, release after use, swap out to the swap area.
- The [[memory-hierarchy]] is defined **from the CPU downwards**: sinking access speed, sinking
  cost, rising capacity, rising access time. [[memory-technologies|SRAM vs. DRAM]] explains why.
- **Internal vs. external [[fragmentation]]** (inside an area vs. between areas) is asked verbatim
  in the exercises.
- [[relocation]] forces the [[physical-and-virtual-memory]] split, which forces
  [[address-translation]], which — because it happens on every access — forces the
  [[memory-management-unit]].
- **Virtual memory = paging + Present/Absent bit.** Everything else (page faults, replacement,
  TLB, working sets) follows from that one bit.
- The [[page-table-entry]] `| C | R | M | Prot | P | frame number |` is the chapter's Rosetta
  stone: P drives [[page-fault|page faults]], M drives write-back ([[dirty-page]]), R drives
  [[clock-page-replacement|Clock]] and [[not-recently-used|NRU]].
- Four [[page-replacement]] algorithms are presented as successively cheaper approximations of
  [[optimal-page-replacement|the optimum]]: LRU → Clock → NRU.
- [[locality]] is the load-bearing assumption of *everything* in the chapter, and the exercises
  test it through array-traversal code.
- The exercise sheets are overwhelmingly **arithmetic**: powers of two, bit-field widths, table
  counts, weighted access times. See [[address-width-calculation]].

## Detailed summary

### Part 1 — Lecture (p01–p17)

**p01–p03, foundations.** The [[memory-hierarchy]] and its four monotone trends, the binary units
(KiB/MiB/GiB), and the RAM/ROM/SRAM/DRAM distinction ([[memory-technologies]]). The four tasks of
[[memory-management]]. The [[cache]]: read hit/miss with a line load and a
Verdrängungskriterium, write with [[write-through-and-write-back]], and multi-level caches.
[[relocation]], the [[physical-and-virtual-memory]] distinction, and the demand that translation
be "very efficient" → [[memory-management-unit]].

**p04–p05, partitioning.** Variant 1 [[static-partitioning]] with its two minuses and one plus,
protected by the [[base-and-limit-register]] pair; the definition of internal and external
[[fragmentation]]. Variant 2 [[dynamic-partitioning]] with its three runtime growth strategies
(adjacent hole / relocate others / room to grow). Variant 2a [[paging]]: equal-sized
[[page-and-frame|frames and pages]], no external fragmentation, expensive translation.

**p06–p08, the paging arithmetic.** Counting frames and pages (1 GiB / 64 KiB / 4 GiB → 16384
frames, 65536 pages). The address split into page number and offset and the rule
`physical = (frame number << m) | offset`. The [[page-table]] as a list of frame numbers indexed
by page number, with its start address in a hardware register reset on every process switch. The
fully worked 16-bit → 12-bit translation example. Then variant 2b: [[virtual-memory]], its three
advantages and one disadvantage, and the term *swapped-out page*.

**p09–p11, the virtual-memory machinery.** [[page-fault]] definition and handling,
[[demand-paging]], the [[dirty-page]] write-back rule. The [[page-table-entry]] bit layout and
the meaning of C/R/M/Prot/P. Why page tables are huge, live in virtual memory themselves, and are
built as [[multi-level-page-table|multi-level tables]] with the `PT1 | PT2 | offset` split.
The [[translation-lookaside-buffer]]: fully associative, one entry per page, hit/miss/fault paths,
LRU replacement, and the modified-bit propagation rule.

**p12–p13, performance.** The three lookup-cost cases (TLB / memory / disk). [[hit-rate]] and its
dependence on [[locality]]; the page-fault rate's dependence on I/O time and locality. The
row-wise vs. column-wise `int[1000][1000]` example. [[working-set]], [[thrashing]], the
[[page-size]] trade-off, and the 68,5 ns weighted-access-time exercise.

**p14–p17, policies.** Fetch policy: [[demand-paging]] vs. [[prepaging]]. Replacement policy:
[[lru-page-replacement|LRU]] with its linked-list implementation and a traced reference string;
[[clock-page-replacement|Clock]] with its Use bit and hand; [[not-recently-used|NRU]] with its
four (R, M) classes. Finally the three refinements: [[page-locking]] (DMA, shared memory),
[[local-and-global-page-replacement]], and [[page-fault-frequency]] as a control loop between
bounds A and B.

### Part 2 — Exercise recap "BS-KVB-Ex 4" (p18–p24)

| # | Question | Wiki page |
|---|---|---|
| 4-1 a | define the memory hierarchy | [[memory-hierarchy]] |
| 4-1 b | internal vs. external fragmentation | [[fragmentation]] |
| 4-1 c | advantages & disadvantage of virtual memory | [[virtual-memory]] |
| 4-1 d | what is a TLB | [[translation-lookaside-buffer]] |
| 4-1 e | what is the Lokalitätsprinzip | [[locality]] |
| 4-2 | row-wise vs. column-wise traversal of `new int[100][100]` | [[locality]] |
| 4-3 | effective access time over cache / memory / disk | [[hit-rate]] |
| 4-4 | translate 53 and 84 in an 8-bit space; spot the page fault | [[page-table]] |
| 4-5 | 2-level table: page size, page count, tables for a 23 MiB process | [[multi-level-page-table]] |
| 4-6 | LRU / NRU / Clock / optimal on one four-frame table | [[optimal-page-replacement]] |

Exercise 4-6 is the standout: the four strategies give **three different victims** (page 1 for LRU
and NRU, page 2 for Clock, page 3 for the optimum), which makes it the single best revision
exercise in the chapter. A margin note there records that the **U bit of Clock is the R bit** of
the page-table entry.

### Part 3 — Bonus sheets "Zusatz 1" and "Zusatz 2" (p25–p28)

**Zusatz 1 (p25–p26)** is pure address arithmetic, and notably *not* about paging: address width
vs. memory size (20-bit and 32-bit byte addressing), **word-wise addressing** with 2 B words, and
two instruction-encoding questions (opcode bits + register bits, byte alignment, and the zero-sum
rule that doubling the instruction set costs one addressable-address bit). All collected in
[[address-width-calculation]].

**Zusatz 2 (p27–p28)** is a five-part two-level paging exercise: page size from the address split,
pages per process, maximum page-table size (257 tables × 8 KiB = 2056 KiB), status bits per entry
(32 − 17 = 15), and the number of main-memory accesses to read one byte (3). See
[[multi-level-page-table]] and [[page-table-entry]].

### Part 4 — "Praxis 4" (p29)

A practical section that belongs thematically to **chapter 3**, not chapter 4: POSIX System V
semaphores (`ipcs -s`, `struct sembuf`, `semop`) and a thread-safe queue built from
`pthread_mutex_*` and two condition variables (`canWrite`, `canRead`). This is the same structure
as [[producer-consumer]], and its content has been folded into [[semaphore]] and [[pthread-api]]
rather than given its own chapter-4 page.

## New pages created

**Topics (2):** [[memory-management]], [[virtual-memory]]

**Concepts (31):** [[memory-hierarchy]], [[memory-technologies]], [[cache]],
[[write-through-and-write-back]], [[relocation]], [[physical-and-virtual-memory]],
[[address-translation]], [[memory-management-unit]], [[static-partitioning]],
[[dynamic-partitioning]], [[base-and-limit-register]], [[fragmentation]], [[paging]],
[[page-and-frame]], [[page-table]], [[page-table-entry]], [[multi-level-page-table]],
[[page-fault]], [[demand-paging]], [[prepaging]], [[dirty-page]],
[[translation-lookaside-buffer]], [[hit-rate]], [[working-set]], [[thrashing]], [[page-size]],
[[page-replacement]], [[page-locking]], [[local-and-global-page-replacement]],
[[page-fault-frequency]], [[address-width-calculation]]

**Algorithms (4):** [[lru-page-replacement]], [[clock-page-replacement]], [[not-recently-used]],
[[optimal-page-replacement]]

## Pages updated

[[overview]], [[index]], [[locality]], [[swapping]], [[process-image]], [[process-switch]],
[[copy-on-write]], [[direct-memory-access]], [[resource]], [[interrupt]], [[trap]],
[[processes]], [[operating-system-basics]], [[semaphore]], [[pthread-api]], [[producer-consumer]]

## Contradictions / conflicts with existing wiki

None. Chapter 4 is largely new territory; where it touches earlier chapters it **extends** them:

- [[locality]] was introduced in ch.01 as an abstract property; ch.04 turns it into the
  measurable [[hit-rate]] and the concrete [[working-set]], and finally into an exam question
  about loop order.
- [[swapping]] was defined in ch.01 as moving a *blocked process's* data out. Ch.04 refines the
  granularity to individual pages ([[virtual-memory]]). These are compatible — whole-process
  swapping and page-level swapping coexist — but the exam wording matters, so both pages now say
  so explicitly.

## Open questions

- **Placement strategies for [[dynamic-partitioning]]** (first-fit, best-fit, worst-fit, buddy
  system) are never mentioned, although the scheme is otherwise fully developed. Either out of
  scope or deferred to chapter 5.
- **Segmentation** (Segmentierung) does not appear anywhere in the notes.
- **FIFO page replacement** is not named, yet exercise 4-6 supplies a "loaded at" column that only
  FIFO would use. Possibly examinable, possibly a leftover.
- **Scheduling strategies** remain absent after four chapters. Chapter 5 is the last chance.
- The **p14 LRU trace** in the notes is only partly legible; the version in
  [[lru-page-replacement]] is a reconstruction and is flagged there.
- The **p21 result of ≈ 600,025 µs** is transcribed as written; the unit bookkeeping in the disk
  term is worth re-checking. Flagged in [[hit-rate]].
- The **p13 line about small pages and page faults** contradicts p12; the direction consistent
  with the rest of the notes has been used, and the conflict is flagged in [[page-size]].

## Sources

- `raw/lecture/bs_4.pdf` (29 pages)
