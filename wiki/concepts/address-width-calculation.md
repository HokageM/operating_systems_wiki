---
title: Address Width Calculation
type: concept
tags: [memory, addressing, exam]
chapter: "04"
sources: [lecture-bs-4]
created: 2026-09-30
updated: 2026-09-30
---

# Address Width Calculation

## Definition

The recurring arithmetic of chapter 4: converting between **memory sizes**, **address widths in
bits**, and **counts of pages, frames, tables and entries**. Every quantity is a power of two, so
every question reduces to adding and subtracting exponents.

## Context

Belongs to [[memory-management]]. It is not a concept of the lecture in its own right, but it is
the skill the exercises (p06–p08, p22, p23, p25–p28) test most often, so it earns a page.

## How it works

### The three rules

```
1.  size = 2^(address width)  *  size of the addressable unit
2.  count = total size / unit size        (subtract the exponents)
3.  width in bits = ceil( log2(count) )   (round UP)
```

Binary prefixes throughout: `1 KiB = 2^10`, `1 MiB = 2^20`, `1 GiB = 2^30`.

### Worked examples

**p25 — 20-bit byte addresses:** `2^20 B = 2^10 MiB = 1024 MiB`.

**p25 — 32-bit byte addresses:** `2^32 B = 2^22 MiB = 4096 MiB = 2^2 GiB = 4 GiB`.

**p25 — word addressing.** Which address width does a processor need at minimum if memory is
addressed **word-wise** (word length 2 B) and 512 MiB is to be managed?

```
512 MiB = 2^9 MiB = 2^29 B
2^29 B / 2 B per word = 2^28 words  ->  28 bit
```

The lesson: **first convert the size into the addressable unit**, then take the logarithm.
Byte-addressing is only the default, not a law.

**p26 — instruction encoding.** A processor with 64 registers and an instruction set of 12
instructions, one instruction = opcode + one register:

```
opcode:   12 instructions -> ceil(log2 12) = 4 bit
register: 64 registers    -> log2 64       = 6 bit
                                  total   = 10 bit
```

A program of 1000 instructions stored **byte-aligned**: 10 bit round up to 2 B, so
`1000 * 2 B = 2000 B`.

**p26 — the zero-sum rule.** Opcode `k` bit + address `n` bit. Doubling the instruction set gives
`(k+1) + (n−1)`: one more opcode bit costs one addressable-address bit, so the **total
instruction length is unchanged** — and the number of addressable addresses halves.

**p28 — status bits in an entry.** `frames = 1 GiB / 8 KiB = 2^17` → 17 bit frame number;
`entry = 4 B = 32 bit`; `32 − 17 = 15 bit` left for status. See [[page-table-entry]].

## Related

- [[page-and-frame]] — the counts being computed.
- [[multi-level-page-table]] — where the splitting of the page number happens.
- [[address-translation]] — what the widths are used for.
- [[memory-hierarchy]] — the unit definitions.

## Common confusions

- **Round *up*, always.** 12 instructions need 4 bits, not 3.6.
- **Byte-aligned ≠ bit-packed.** 10-bit instructions occupy 2 B each, not 1.25 B.
- **The addressable unit matters.** Word addressing with 2 B words halves the address width needed
  for the same memory size.
- **Bits vs. count.** "How many bits for the frame number" and "how many frames" differ by a
  `log2`; exercises alternate between the two phrasings within the same question.

## Sources

- [[lecture-bs-4]] (p06, p07, p22, p23, p25, p26, p27, p28)
