---
title: Nested RAID (0+1 and 1+0)
type: concept
tags: [file-system, storage, raid]
chapter: "05"
sources: [lecture-bs-5]
created: 2026-09-30
updated: 2026-09-30
---

# Nested RAID (0+1 and 1+0)

## Definition

Combinations of [[raid-0]] and [[raid-1]], obtained by applying one level on top of the other. The
order matters, and the names encode it **bottom level first**:

| Name | Structure |
|---|---|
| **RAID 0+1 (01)** | **two disks form a RAID 0**, which is then **mirrored** |
| **RAID 1+0 (10)** | **RAID 1 at the lowest level**, several of which are combined into a **RAID 0** |

```
RAID 0+1                          RAID 1+0
      mirror                            stripe
     /      \                          /      \
 stripe    stripe                  mirror    mirror
 /    \    /    \                  /   \     /    \
d0    d1  d2    d3               d0    d1   d2    d3
```

## Why the order matters

Both give speed **and** failure safety at **50 % net capacity**, but they behave differently after
a failure:

- In **1+0**, a failed disk kills only its own mirror pair; the array survives, and it can even
  survive several failures as long as they hit different pairs. Rebuilding copies **one disk**.
- In **0+1**, a failed disk kills its whole stripe set; the array now depends entirely on the other
  stripe set. Rebuilding has to reconstruct **the whole side**.

RAID 1+0 is therefore preferred in practice. The notes give only the structures, but the
structures are enough to derive this.

## Context

Belongs to [[raid]]. Nesting is the brute-force way to get both goals; [[raid-5]] achieves the
same protection at (n−1)/n capacity instead of 1/2, which is why nesting is reserved for workloads
where the write penalty of parity is unacceptable.

## Related

- [[raid-0]], [[raid-1]] — the components.
- [[raid-5]], [[raid-6]] — the parity-based alternatives.
- [[raid-parity-update]] — the cost that nesting avoids entirely: mirroring needs no
  read-modify-write.

## Sources

- [[lecture-bs-5]] (p23)
