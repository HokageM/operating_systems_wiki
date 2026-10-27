---
title: Scheduling Goals
type: concept
tags: [process, scheduling]
chapter: "05"
sources: [lecture-bs-5]
created: 2026-09-30
updated: 2026-09-30
---

# Scheduling Goals

## Definition

The criteria against which a [[scheduling]] strategy is judged. The notes deliberately split them
into **the user's goals** and **the operating system's goals**, because the two sets conflict.

## The two lists

**From the user's point of view:**

- **short response times for interactive jobs** (kurze Antwortzeiten für interaktive Aufträge)
- **long dwell times for batch jobs** are acceptable (lange Verweilzeiten bei Stapelaufträgen)

**From the operating system's point of view:**

- **high throughput** (hoher Durchsatz)
- **high resource utilisation** (hohe Auslastung der Betriebsmittel)
- **fairness** (Fairness)
- **little overhead** (geringer Overhead)

## Context

Belongs to [[scheduling]]. These goals are what the algorithms of the chapter trade against each
other: [[shortest-process-next]] optimises mean waiting time and sacrifices fairness;
[[round-robin]] optimises fairness and response time and sacrifices mean waiting time;
[[first-come-first-served]] optimises overhead and sacrifices everything else.

## Why they conflict

- **Response time vs. overhead.** Short response times require a small [[time-slice]], and a small
  time slice means many [[process-switch|process switches]] — pure overhead.
- **Throughput vs. fairness.** Throughput is maximised by always running the job that finishes
  soonest; fairness requires giving time to jobs that will not finish soon.
- **Utilisation vs. response time.** Admitting more processes raises utilisation but lengthens
  every queue — the same tension as over-commitment in [[virtual-memory]] (see [[thrashing]]).

## Related

- [[scheduling]] — the topic.
- [[cpu-utilisation]] — the concrete metric behind "high resource utilisation".
- [[preemptive-and-non-preemptive-scheduling]] — the mechanism that buys response time at the
  price of overhead.
- [[starvation]] — the total failure of fairness.

## Exam relevance

Exercise S-1 a) asks directly: *"What is the decisive performance requirement in an interactive
operating system?"* → **short response times for interactive jobs**. The trap is to answer
"throughput", which is the *system's* goal, not the interactive user's.

## Sources

- [[lecture-bs-5]] (p01, exercise S-1 a on p28)
