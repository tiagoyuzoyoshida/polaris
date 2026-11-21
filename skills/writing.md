# Skill: Human-Facing Writing

Rules for anything written for human eyes: `README.md`, `SYSTEM.md`, `DEBRIEF.md`, `LOGS.md`, and user-facing documentation.

## Core principle: communicate only the essential ideas

Good writing is defined as much by what it **omits** as by what it includes.

- **Omission is intentional.** Leaving something out loses information, and that is the point when the detail does not serve what *this* document needs to communicate to *this* reader.
- **Filter by reader context, not completeness.** Do not mention a technology, library, internal abstraction, or design decision simply because the project uses it. Include a fact only if the reader needs it to understand, use, or act on the document.
- **Every sentence must earn its place.** Once the essential ideas are chosen, express them with the fewest words that stay clear and accurate. If a sentence repeats another, pads a transition, or adds no decision-relevant meaning, cut it.

## Structure

1. **Lead with what matters.** Put the core takeaway, purpose, or answer in the first two lines. Context and details come after.
2. **One document, one job.** Do not turn an overview into a reference manual, a `README.md` into an architecture dump, or a `DEBRIEF.md` into a changelog of file edits.
3. **Make it scannable.** Use short paragraphs (2–4 lines max), clear headings, bullet lists for parallel items, and tables for structured comparisons. Never write walls of prose.
4. **Define terms on first use.** Assume the reader has not opened the codebase in weeks. Name parts consistently with `SYSTEM.md`.

## Voice and style

- **Plain, direct statements.** Prefer concrete verbs and specific facts over vague qualifiers.
- **Present tense** for how the system works today; past tense only for history (`LOGS.md`, `DEBRIEF.md`).
- **Zero filler.** Cut throat-clearing ("In this section we will..."), hedging ("simply", "just", "essentially", "basically"), marketing adjectives ("powerful", "seamless", "robust", "cutting-edge"), and self-congratulation.
- **No emojis** unless the repository already uses them consistently.
- **Show, don't narrate.** Use a minimal runnable command or example instead of three sentences describing what a command does.

## File-specific guidance

### `README.md`

A `README.md` answers four questions for someone landing on the repository: *What is this? Why should I care? How do I run it? Where do I go next?*

- **Opening:** One-line description of what the project does and for whom. No tagline fluff.
- **Value / core idea:** Only what differentiates it or explains why it exists. Do not list implementation choices (language, framework, internal patterns) unless they directly matter to the user.
- **Quickstart:** Minimal steps that work from a fresh clone, plus one verified example showing input and result.
- **Onward links & license:** Short pointers to deeper docs if they exist, and the license. Do not duplicate full configuration tables, CLI flags, or directory trees here unless the README *is* the only doc.

### `SYSTEM.md`

- Explain the mental model of the system as built today, not the history of how it was built or what is planned next.
- Keep prose short; let the diagrams carry structure and flows, and use text only to explain *why* parts exist or what the diagram cannot show.

### `DEBRIEF.md`

- **TL;DR first** (at most 5 lines): what the system can do now that it could not before, and anything the human needs to decide.
- Explain behavior, interactions, and reasoning at the level of system logic — never restate the checklist, list edited files line by line, or paste code.

### `LOGS.md`

- At most three short bullets per cycle: what was delivered, how it went (blocks/revisions and cause), and any milestone/amendment/`LATER.md` change. Never explain how it works.

## Self-check before finishing

1. What is the single main thing this reader needs from this document?
2. For every section and sentence: *would this reader be worse off without it?* If not, cut it — even if the fact is true.
3. Are there any internal implementation choices mentioned only because they exist in the code, not because the reader needs them? Cut them.
4. Can any paragraph be cut in half without losing the essential idea? Tighten it.
