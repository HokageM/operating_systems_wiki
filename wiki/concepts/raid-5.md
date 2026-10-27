---
title: RAID 5
type: concept
tags: [file-system, storage, raid]
chapter: "05"
sources: [lecture-bs-5]
created: 2026-09-30
updated: 2026-09-30
---

# RAID 5

## Definition

RAID 5 **remedies the disadvantage of [[raid-4]] by distributing the parity information over ALL
disks**, so that **write accesses can also be carried out in parallel**. Everything else — block
-wise XOR parity, tolerance of exactly one disk failure, net capacity (n−1)/n — is unchanged.

## Layout

The parity block **rotates** from stripe to stripe (the diagram on p25, for three disks):

```
stripe    disk 0   disk 1   disk 2
  0        DA       DB       PAB
  1        DC       PCD      DD
  2        PEF      DE       DF
  3        DG       DH       PGH
```

Because no disk is the parity disk, two writes to different stripes usually hit different disks and
proceed **in parallel**. The bottleneck of RAID 4 is gone.

## Address mapping

Exercise S-5 2) (p34) asks for the mapping with *n* disks. With n−1 data blocks per stripe:

```
stripe  = A / (n-1)
index   = A mod (n-1)            position among the data blocks of the stripe
parity disk of the stripe = (n-1) - (stripe mod n)     (left-symmetric rotation)
disk    = index, shifted past the parity disk
block address on the disk = stripe
```

The exact rotation direction is a convention; what the exercise tests is that you (a) divide by
**n−1**, not *n*, because one block per stripe is parity, and (b) shift the data blocks past the
rotating parity position.

## Pros and cons

| | |
|---|---|
| (+) | tolerates the failure of **one** disk |
| (+) | **net capacity (n−1)/n** |
| (+) | reads **and writes** in parallel |
| (−) | the **small-write penalty** remains — see [[raid-parity-update]] |
| (−) | rebuilding after a failure reads **every** remaining disk in full |

## Context

Belongs to [[raid]]. RAID 5 is the default choice for general-purpose redundant storage: it is the
best compromise the chapter offers between the capacity of [[raid-0]], the safety of [[raid-1]] and
the write throughput of neither.

## Related

- [[raid-4]] — identical except for the parity placement; the comparison is the exam question.
- [[raid-6]] — the extension to two-disk tolerance.
- [[parity-and-xor]], [[raid-parity-update]] — the arithmetic and its cost.
- [[hot-spare-and-hot-swap]] — because RAID 5 tolerates only one failure, the window until the
  rebuild finishes matters, which is exactly what a hot-spare disk shortens.

## Sources

- [[lecture-bs-5]] (p25, exercise S-5 on p34)
