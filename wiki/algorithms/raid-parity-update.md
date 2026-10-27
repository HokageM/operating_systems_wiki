---
title: RAID Parity Update (Additive and Subtractive)
type: algorithm
tags: [file-system, storage, raid]
chapter: "05"
sources: [lecture-bs-5]
created: 2026-09-30
updated: 2026-09-30
---

# RAID Parity Update (Additive and Subtractive)

## Purpose

When **one** data block in a parity-protected stripe ([[raid-4]], [[raid-5]], [[raid-6]]) is
overwritten, the stripe's [[parity-and-xor|parity block]] becomes stale and must be recomputed.
There are **two methods**, and choosing the cheaper one is what exercise S-6 tests.

This is the origin of the famous **small-write penalty**: a single logical write of one block turns
into several physical reads and writes.

## Procedure

Let the stripe have *n* disks, so **n−1 data blocks** D₀ … D_{n−2} and one parity block P. Block
D_i is being overwritten with D_i'.

### Method 1 — additive (recompute from scratch)

```
1. read all the OTHER data blocks:  D_j for all j != i        (n-2 reads)
2. P' = D_i' XOR (XOR over all D_j)
3. write D_i' and P'                                          (2 writes)
```

Cost: **n − 2 reads + 2 writes**. It grows with the width of the array.

### Method 2 — subtractive (read-modify-write)

```
1. read the OLD data block D_i and the OLD parity P           (2 reads)
2. P' = P XOR D_i XOR D_i'
3. write D_i' and P'                                          (2 writes)
```

Cost: **2 reads + 2 writes**, **independent of n**.

The correctness follows from the identity in [[parity-and-xor]]: `(a XOR b) XOR a = b`. XOR-ing the
old data out of the parity removes exactly its contribution, leaving the parity of all the *other*
blocks; XOR-ing the new data in restores a correct parity — without ever reading those other
blocks.

## Worked example

Exercise S-6 (p35). For an array of, say, **n = 5** disks (4 data + 1 parity), updating one block:

| Method | reads | writes | total I/Os |
|---|---|---|---|
| additive | n − 2 = 3 | 2 | **5** |
| subtractive | 2 | 2 | **4** |

For **n = 4**: additive 2 + 2 = 4, subtractive 2 + 2 = 4 — **equal**. For **n = 3**: additive
1 + 2 = 3, subtractive 4 — the **additive** method wins.

So the rule is: **subtractive for wide arrays, additive for narrow ones**, with the break-even at
n = 4. Real controllers choose per request, and also use the additive method when the whole stripe
is being rewritten anyway (then no reads are needed at all).

## Complexity / cost

| | I/Os |
|---|---|
| additive | **n** (n−2 reads + 2 writes) |
| subtractive | **4**, constant |
| full-stripe write | **n** writes, **0 reads** — the cheapest per byte |

The last row is why sequential, stripe-aligned writing is so much faster on a parity RAID than
random small writes: the parity comes for free.

## Pros and cons

| | Additive | Subtractive |
|---|---|---|
| (+) | better for **small n**; no read of the old parity | **constant cost**, independent of n |
| (−) | cost **grows with n** | always 4 I/Os, even when n is tiny |

## Compared to

[[raid-1]] needs **no parity update at all** — a write is just *n* writes, no reads. That is the
performance argument for [[nested-raid]] over [[raid-5]] on write-heavy workloads, and it is worth
half the capacity to some people.

## Related

- [[parity-and-xor]] — the identity that makes the subtractive method work.
- [[raid-4]], [[raid-5]], [[raid-6]] — where it is applied. RAID 6 pays it twice.
- [[raid-consistent-update-problem]] — the data and parity writes in step 3 must be atomic.
- [[disk-access-time]] — each of those I/Os costs a seek plus a rotation.

## Sources

- [[lecture-bs-5]] (exercise S-6 on p35)
