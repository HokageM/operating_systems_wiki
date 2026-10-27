---
title: Process State Model
type: concept
tags: [process, states]
chapter: "01"
sources: [lecture-bs-1, lecture-bs-5]
created: 2026-09-30
updated: 2026-09-30
---

# Process State Model

## Definition

The **process state model** (Prozesszustandsmodell) **describes the possible states of a
[[process]] and all possible transitions between them**.

## Context

Belongs to [[processes]]. Three versions are taught, each adding detail the previous one could not
express.

## How it works

### Simple model (2 states)

```
New --Admit--> Not Running --Dispatch--> Running --Release--> Exit
                    ^                        |
                    +--------Pause-----------+
```

### Extended model (5 states)

```
New --Admit--> Ready --Dispatch--> Running --Release--> Exit
                 ^                    |
                 +-----Timeout--------+
                 ^                    |
        Event Occurs             Event Wait
                 |                    v
               Blocked <--------------+
```

The addition is **Blocked**: a running process that waits for an event (I/O completion, a
[[signal]], data in a [[pipe]]) is not *ready*, so the [[scheduler]] must not pick it.

### 7-state model (with suspend states)

[[swapping]] adds **Ready/Suspend** and **Blocked/Suspend** for processes whose data has been moved
out to persistent storage:

- New → Ready **or** Ready/Suspend (Admit)
- Ready/Suspend ↔ Ready (Activate / Suspend)
- Ready --Dispatch--> Running
- Running --Timeout--> Ready · Running --Suspend--> Ready · Running --Release--> Exit
- Running --Event Wait--> Blocked · Blocked --Event Occurs--> Ready
- Blocked ↔ Blocked/Suspend (Suspend / Activate)
- Blocked/Suspend --Event Occurs--> Ready/Suspend

## Related

- [[swapping]] — the reason the suspend states exist.
- [[scheduler]] — chooses among the *Ready* processes.
- [[dispatcher]] — performs the Ready → Running transition.
- [[process-switch]] — the machinery behind a transition out of Running.
- [[thread-state-model]] — the same ready/blocked logic per [[thread]], given as four transitions.
- [[deadlock]] — a set of processes permanently stuck in **Blocked**.
- [[livelock]] — processes cycling between **Ready** and **Running** without progress; the
  passive/active contrast with deadlock is defined on exactly these states.
- [[mutex]], [[semaphore]] — what moves a process into Blocked in chapter 3.
- [[preemptive-and-non-preemptive-scheduling]] — preemption is the **Running → Ready** transition; a non-preemptive policy never takes it.
- [[ready-queue]] — the concrete data structure behind the state *Ready*.
- [[clock-interrupt]] — what forces the Running → Ready transition in a time-slice based system.

## Common confusions

- **Ready vs. Blocked.** Ready means "only the CPU is missing"; Blocked means "an event is
  missing". A blocked process cannot be dispatched however free the CPU is.
- **Blocked/Suspend --Event Occurs--> Ready/Suspend**, not → Ready: the event arriving does not
  bring the process's data back into memory.
- **Timeout goes to Ready, Event Wait goes to Blocked** — mixing these two transitions up is the
  most common diagram error.

## Sources

- [[lecture-bs-1]] (p10, p11)
- [[lecture-bs-5]] (p01–p08) — preemption as a state transition; the ready queue made concrete.
