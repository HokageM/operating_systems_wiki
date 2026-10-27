---
title: Parity and XOR
type: concept
tags: [file-system, storage, raid]
chapter: "05"
sources: [lecture-bs-5]
created: 2026-09-30
updated: 2026-09-30
---

# Parity and XOR

## Definition

**Parity information** is redundancy computed **block-wise with XOR** over the data blocks of a
stripe. It allows any **one** missing block of the stripe to be reconstructed, at a cost of one
extra block instead of a full copy.

For a stripe of data blocks D₀ … D_{n−2}:

```
P = D0 XOR D1 XOR ... XOR D(n-2)
```

## Why it works

XOR is its own inverse and is associative and commutative, so XOR-ing the parity with all the
surviving blocks yields the missing one:

```
D1 = P XOR D0 XOR D2 XOR ... XOR D(n-2)
```

The identity the notes single out is the two-term case:

```
(a XOR b) XOR a = b
```

This is the foundation of the **subtractive parity method** in [[raid-parity-update]]: the old
data block can be "removed" from the parity without reading any of the other blocks.

## Why parity beats mirroring on capacity

| | Redundancy per stripe | Net capacity |
|---|---|---|
| [[raid-1]] | one full copy per disk | 1/n |
| [[raid-4]] / [[raid-5]] | **one block** | **(n−1)/n** |
| [[raid-6]] | **two blocks**, from **two different computations** | **(n−2)/n** |

The price is paid on writes rather than on capacity — see [[raid-parity-update]].

## Context

Belongs to [[raid]]. It is what distinguishes levels 4, 5 and 6 from levels 0 and 1: redundancy
computed rather than copied.

## Related

- [[raid-4]] — parity on a dedicated disk.
- [[raid-5]] — parity distributed over all disks.
- [[raid-6]] — two independent parity computations, hence two-disk tolerance.
- [[raid-parity-update]] — the two ways to keep parity current on a write.
- [[atomic-operation]] — parity is only meaningful if the data and parity writes happen together;
  see [[raid-consistent-update-problem]].

## Common confusions

- **Parity detects nothing.** It repairs a block known to be missing (a failed disk announces
  itself). It cannot tell you *which* block is wrong if one silently returns bad data — that needs
  checksums, which RAID does not provide.
- **One parity block protects against one failure only.** Two simultaneous failures in a stripe
  leave two unknowns and one equation. Hence [[raid-6]].

## Sources

- [[lecture-bs-5]] (p24, p26, exercise S-6 on p35)
