---
title: Disk Access Time
type: concept
tags: [file-system, storage, performance]
chapter: "05"
sources: [lecture-bs-5]
created: 2026-09-30
updated: 2026-09-30
---

# Disk Access Time

## Definition

The time to read or write data on a mechanical hard disk is the sum of three parts:

```
access time = seek time + rotational delay + transfer time
                                             (amount of data / transfer rate)
```

- **seek time** (Suchzeit) — moving the arm to the right track.
- **rotational delay** (Drehverzögerung) — waiting for the right sector to come round.
- **transfer time** — the only part proportional to the amount of data.

The first two are **mechanical, fixed, and paid once per request**. Everything interesting about
disk performance follows from that.

## Worked example

Exercise S-4 (p33). Given: average seek time **7 ms**, average rotational delay **3 ms**, transfer
rate **50 MiB/s**.

**a) Sequential (S)** — one request of 10 MiB:

```
access time = 7 ms + 3 ms + 10 MiB / 50 MiB/s = 7 + 3 + 200 = 210 ms
S = 10 MiB / 210 ms = 47.62 MiB/s
```

**b) Random (R)** — requests of 10 KiB each:

```
access time = 7 ms + 3 ms + 10 KiB / 50 MiB/s = 7 + 3 + 0.195 = 10.195 ms
R = 10 KiB / 10.195 ms = 0.958 MiB/s
```

**c) IOPS** — operations per second when the transfer time is negligible:

```
IOPS = 1 operation / (seek time + latency) = 1 / (7 ms + 3 ms) = 100 operations/s
```

## The punchline

The **same hardware** delivers **47.62 MiB/s sequentially and 0.958 MiB/s randomly** — a factor of
about **50**. The rated "50 MiB/s" is reached only in the sequential case, and even then only
approached; in the random case 98 % of the time is spent positioning.

This single number is the quantitative justification for most of the chapter:

- why [[locality]] and [[file-access-types|sequential access]] matter at all;
- why [[disk-fragmentation]] is worth defragmenting;
- why [[disk-block|blocks]] are large and why [[contiguous-allocation]] performs so well;
- why [[linked-list-allocation]]'s pointer chase is described as *very slow*;
- why [[raid-0]] striping pays off;
- and why the [[solid-state-disk]], which has neither seek time nor rotational delay, changed
  storage design completely.

## Related

- [[solid-state-disk]] — removes the first two terms.
- [[file-access-types]] — the qualitative version of this page.
- [[locality]], [[disk-fragmentation]], [[disk-block]] — the design consequences.
- [[hit-rate]] — the same style of effective-access-time calculation as in chapter 04. Both
  exercises reward writing the formula down before substituting numbers.
- [[memory-hierarchy]] — this is the measurement of its slowest level.

## Common confusions

- **IOPS and MiB/s measure different things.** IOPS counts *requests*, throughput counts *bytes*. A
  disk can be at 100 % of its IOPS limit and at 2 % of its rated throughput at the same time —
  which is exactly case b).
- **"Average" seek time is an average over a random access pattern.** A sequential workload sees
  almost no seeks at all, which is why case a) pays only one.

## Sources

- [[lecture-bs-5]] (exercise S-4 on p33)
