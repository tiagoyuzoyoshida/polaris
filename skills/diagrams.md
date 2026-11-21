# Skill: Diagrams and Visual Explanations

Rules for creating clear, intuitive Mermaid diagrams and pairing them with minimal prose (in `SYSTEM.md`, docs, and deep-dive explanations).

## Core principle: every element must serve the communication

A diagram is a **model**, not a photograph of the codebase. Its value comes from what it leaves out.

- **One question per diagram.** Decide the single question the diagram answers (for example, *"What are the main parts and how do they connect?"* or *"What happens when a request arrives?"*). Omit anything that does not help answer that question — losing detail is intentional.
- **Concept-level, not file-level.** Boxes represent responsibilities, subsystems, actors, or stores — never a 1:1 mirror of source files, helper functions, or boilerplate layers.
- **Every node, edge, and group must earn its place.** If removing a box or arrow leaves the reader with the same mental model, remove it.

## Choose the right diagram type

Pick the Mermaid type that matches the question:

| Question | Diagram type | Use for |
|---|---|---|
| What are the parts and dependencies? | `flowchart LR` or `flowchart TD` | System maps, data/control pipelines, boundaries |
| What happens in what order across actors? | `sequenceDiagram` | End-to-end request/event flows, handoffs |
| What states can something be in and what triggers transitions? | `stateDiagram-v2` | Lifecycles, loop/job states, protocols |
| What are the core domain entities and relationships? | `erDiagram` | Data models (only when schema itself is the point) |

Never cram architecture, time order, and error handling into one chart. Use two small diagrams instead of one dense diagram.

## Visual grammar and layout

1. **Node budget: 5–9 nodes per diagram.** Past ~10 nodes, readers stop seeing structure. Either raise the abstraction level or group related nodes into 2–4 `subgraph` blocks.
2. **Consistent flow direction.** Prefer `flowchart LR` for pipelines and system maps (inputs on the left, outputs/stores on the right) and `flowchart TD` for hierarchies. Avoid arrows that loop backward unless representing an actual feedback/retry cycle.
3. **Distinguish roles by shape** (use consistently across the document):
   - Actors / external systems: stadium or rounded `(["User"])`
   - Processes / components: rectangle `["Planner"]`
   - Data stores / files: cylinder `[("Database")]`
   - Decisions (only when branching is essential): diamond `{"Valid?"}`
4. **Label every edge with an active verb.** Arrows explain *what flows or happens* (`-->|"reads state"|`, `-->|"returns diff"|`), never bare unlabeled arrows unless the relationship is unmistakable. Keep edge labels to 1–3 words.
5. **Short, consistent node labels.** 2–4 words per node. Use the exact part names from `SYSTEM.md` so the diagram and prose reinforce each other.
6. **Minimize crossing lines.** Order declarations in flow order (source to sink) so Mermaid lays out clean paths instead of a spiderweb.

## Mermaid syntax hygiene

- **Quote labels containing special characters** (parentheses, colons, brackets, slashes, commas): `api["API Gateway (HTTP)"]`, not `api[API Gateway (HTTP)]`.
- **Avoid raw HTML tags** (`<br/>`, `<b>`) in node labels unless strictly necessary; shorter labels are almost always better.
- **Use simple alphanumeric IDs** (`planner`, `db`, `auth_svc`) separate from display labels.
- **In `sequenceDiagram`**, declare `participant` aliases at the top in left-to-right interaction order, and limit `alt`/`opt` blocks to the one or two branches that matter to the reader.

## Pairing diagrams with explanations

- **Do not narrate the diagram.** Never follow a diagram with prose that merely recites every box and arrow ("A sends X to B, then B sends Y to C").
- **Use prose for what the diagram cannot show:** *why* a boundary exists, what invariant holds across it, or what happens when a key assumption fails.
- **One caption or framing line before the diagram** stating what question it answers, if the heading alone does not make it obvious.

## Anti-patterns to reject

- **The file-tree dump:** every module/file drawn as a box with import arrows.
- **The spiderweb:** >12 nodes with crossing edges and no grouping.
- **The mystery arrow:** `A --> B` with no label and no obvious meaning.
- **The kitchen sink:** mixing happy path, every error path, logging, and config loading in a single chart.
- **The orphan label:** names in the diagram that do not match the names used in `SYSTEM.md` or `DEBRIEF.md`.

## Self-check before finishing

1. Can a reader grasp the main idea in under 10 seconds?
2. Does the diagram answer one clear question, with everything unrelated omitted?
3. Are there ~5–9 nodes (or cleanly grouped subgraphs), active verb labels on edges, and consistent shapes?
4. Do all labels match `SYSTEM.md` part names, and is every label with special characters properly quoted?
