---
title: Resource (Betriebsmittel)
type: concept
tags: [basics, definition]
chapter: "01"
sources: [lecture-bs-1, lecture-bs-4, lecture-bs-5]
created: 2026-09-30
updated: 2026-09-30
---

# Resource (Betriebsmittel)

## Definition

A **resource** (Betriebsmittel) is any **allocatable hardware or software component of a computing
system**. Managing resources and controlling their allocation is clause 1 and 2 of the definition
of an [[operating-system]].

## Context

Belongs to [[operating-system-basics]]. The word is deliberately broad: CPU time, main memory,
disk blocks, I/O devices, but also software objects such as files, open connections and
semaphores.

## How it works

The OS keeps a management structure per resource class — the lecture names the **primary process
table**, the **memory table** (Speichertabelle) and the **I/O table**. Allocation is *controlled*:
a process obtains a resource only by asking the OS via a [[system-call]], never by grabbing it
directly (which is why [[user-mode-and-kernel-mode]] exists).

## Related

- [[operating-system]] — defined in terms of this word.
- [[process]] — the unit to which resources are allocated and accounted.
- [[device-driver]] — how a hardware resource is made usable at all.
- [[consumable-and-reusable-resources]] — the chapter-3 taxonomy: resources that are used up
  versus resources that are returned unchanged.
- [[conflict]] — what happens when two processes access the same resource concurrently.
- [[deadlock]] — the failure mode of allocating resources exclusively.
- [[semaphore]] — a counting semaphore models *n* interchangeable instances of a resource.
- [[memory-management]] — main memory is the resource of chapter 4, and
  [[page-and-frame|frames]] are its interchangeable instances.
- [[file-systems]] — persistent storage as the fourth managed resource, after CPU ([[scheduling]]), memory and devices.

## Common confusions

- **Not just hardware.** The German term is often read as "devices"; the definition explicitly
  includes software components.
- **Allocatable is the key adjective** — something that cannot be handed out to a requester is not
  a Betriebsmittel.
- **The two great resources of the module are CPU time and main memory.** Chapters 1–3 allocate
  the first ([[scheduler]], [[process-switch]]); chapter 4 allocates the second
  ([[memory-management]], [[page-replacement]]). The vocabulary is deliberately parallel —
  allocation, release, [[starvation]], fairness.

## Sources

- [[lecture-bs-1]] (p01)
- [[lecture-bs-5]] (p11) — the file management system as a resource manager.
