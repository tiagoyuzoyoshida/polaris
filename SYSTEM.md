# SYSTEM.md: <project name>, System Overview

A visual, plain-language guide to how the system works today, written for the human. `PROJECT.md` is the intent, `MEMORY.md` holds agent working memory, and this file is the human's high-level view. Maintained by the aide at every close from the actual code. Nothing planned or aspirational appears here.

## 1. Snapshot

- **Does today:** <what the system can actually do, end to end, in two or three plain lines>
- **Milestone:** <current milestone ID and name, and how far along>

## 2. System map

<Required `mermaid` flowchart or graph showing the main parts, data stores, and external systems, and how they connect.>

```mermaid
flowchart LR
  <nodes and edges>
```

## 3. Key flows

<Required `mermaid` sequence or flow diagrams for the main end-to-end scenarios (one to three diagrams): what triggers each, what happens in order, and what comes out.>

### 3.1 <scenario>

```mermaid
sequenceDiagram
  <participants and steps>
```

## 4. Parts at a glance

| Part | What it does | Where it lives |
|---|---|---|
| <part> | <one plain line> | `<path>` |

## 5. Not real yet

<Everything stubbed, faked, hardcoded, or known to be limited, with the `LATER.md` ID that tracks it, or `None`.>
