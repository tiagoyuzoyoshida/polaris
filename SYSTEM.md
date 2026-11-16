# SYSTEM.md: <project name>, System Design As Built

The design of the system that exists in this repository today. `PROJECT.md` is the intent, and this file is the reality. Maintained by the planner under `POLARIS.md` section 6.5 and verified against the code at every close. Nothing planned or aspirational appears here.

## 1. Snapshot

- **Does today:** <what the system can actually do, end to end, in two or three lines>
- **Milestone:** <current milestone ID and name, and how far along>

## 2. System map

<One text diagram of every component, process, data store, and external system, with the data or calls that flow between them.>

## 3. Components

<One subsection per box in the map, in dependency order.>

### 3.1 <component>

- **Responsibility:** <the one thing it owns>
- **Interface:** <what it accepts and returns, and who calls it>
- **State:** <what it owns and persists, or `None`>
- **Depends on:** <components and external systems>
- **Code:** `<paths>`

## 4. Data model

<The key structures, schemas, and formats that cross component boundaries: what each represents, its fields with units, and which component owns it.>

## 5. Flows

<The main end-to-end scenarios, each as numbered steps through the components: what triggers it, what happens in order, and where it can fail.>

### 5.1 <scenario>

1. <step>

## 6. Runtime

<How it executes: processes and threads, what runs where, timing and rates, configuration that changes behavior, and external services.>

## 7. Design decisions

<The choices that shape the system and why each was made.>

| Decision | Why | Record |
|---|---|---|

## 8. Not real yet

<Everything stubbed, faked, hardcoded, or known to be limited, with the `LATER.md` ID that tracks it.>
