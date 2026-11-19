# AGENT.md: Engineering Rules

Universal rules for writing code in any repository. They say nothing about what a project is. Project facts live in `polaris/PROJECT.md` and `docs/`.

**Precedence.** Hard rules (section 1) are never broken. Project files and consistent existing repo conventions override the defaults (sections 2 to 8). Otherwise a default is overridden only for a reason specific to the code at hand (a measurement, a framework constraint), stated in a comment at the site. "It is cleaner" is not a reason.

## 1. Hard rules

1. **Verify, never assume.** Do not invent APIs, flags, paths, signatures, or results. Read the source or the docs, or run the code. Nothing is done until section 9 has been executed. Say plainly what you could not verify.
2. **Stay in scope.** Change only what the task requires. No drive-by refactors, renames, reformatting, or unrequested features. Every changed line must trace directly to the task. Report adjacent problems and pre-existing dead code instead of touching them.
3. **Fail loudly.** Never swallow an error, substitute a silent default, or return a placeholder when an invariant breaks. Raise a specific, actionable error.
4. **Never weaken a check to pass it.** Do not delete, skip, loosen, or mock away a failing test, lint rule, or type error. Fix the cause or report it.
5. **One source of truth.** Every fact, constant, and piece of logic lives in one place, and everything else refers to it. A change that makes a doc stale fixes or deletes that doc in the same change.
6. **Leave no debris.** No scratch files, debug output, commented-out code, dead branches, unused parameters, or `_old`/`_v2` copies. Remove imports, variables, and functions that your changes made unused, without deleting pre-existing dead code. Version control holds history.
7. **No secrets or machine-specific values in the repo.** No credentials, tokens, hosts, absolute paths, or user settings. Use documented configuration and environment variables.
8. **Confirm before destroying.** Deleting data, rewriting history, force-pushing, or removing files you did not create requires explicit approval.
9. **Think before coding.** State assumptions explicitly and surface tradeoffs. If multiple interpretations exist, present them instead of picking silently. If something is unclear or a wrong guess is costly to undo, stop and ask. Otherwise take the simplest reading and state the assumption.

## 2. Method

1. **Orient first.** Read the layout, the project files, the nearest similar code, its tests, and the tooling config before writing anything.
2. **Match the codebase.** New code is indistinguishable from the best existing code around it: same error handling, naming, structure, and test style.
3. **Reuse before creating.** Search for an existing function, type, or pattern first. A new file, abstraction, dependency, or directory needs a concrete reason.
4. **No dependency** for what the standard library or the existing stack already provides. Constrain versions the way the project does.
5. **Fix causes, not symptoms.** A bug fix starts with a test that reproduces the bug, then removes its cause.
6. **Make the smallest change that fully solves the problem,** then stop. No speculative flexibility or unrequested configurability. If 200 lines could be 50, rewrite it; ask whether a senior engineer would call it overcomplicated.

## 3. Design

1. **One responsibility.** A function, class, or module does one cohesive thing. Parsing, I/O, state mutation, and core logic do not share a scope.
2. **Pure core, effects at the edges.** Core logic transforms explicit inputs into explicit outputs. I/O, network, logging, clocks, and randomness live at the boundary and are passed in.
3. **Dependencies flow one way.** Lower layers never import higher ones, and there are no cycles. Framework objects (CLI namespaces, requests, config objects) are unpacked into plain values at the boundary and never reach core logic.
4. **Concrete before abstract.** Write the direct solution. An interface, base class, registry, or generic layer appears only when two real implementations exist.
5. **Make invalid states unrepresentable.** Use the strongest typing the language offers. Named structured types cross module boundaries, never loose dictionaries or positional tuples. Enumerations replace string and boolean flags. Public signatures are annotated.
6. **Validate at the boundary, trust inside.** Check user input, files, network payloads, and configuration where they enter. Inside, rely on the types and contracts.
7. **Handle errors where you can act on them.** Catch narrowly, add context, re-raise what you cannot handle. Release resources deterministically (`with`, `defer`, RAII, `finally`).
8. **Explicit and deterministic.** No hidden global state, import side effects, or action at a distance. Randomness is seeded, and results are reproducible from committed inputs.
9. **Minimal public surface.** Everything is private until something outside needs it. Public names and module paths are stable.
10. **Performance follows measurement.** Avoid plainly wasteful complexity, optimize only what a measurement indicts, and record the measurement where the optimization lives.

## 4. Code

1. **Names carry meaning.** Encode units, frames, shapes, and time bases wherever a mix-up would be silent (`timeout_ms`, `mass_kg`, `pose_world_from_cam`). No vague names (`data`, `tmp`, `handle`, `manager`) and no catch-all modules (`utils`, `helpers`, `common`).
2. **No magic values.** Unexplained literals become named constants or typed configuration, with units.
3. **Flat control flow.** Guard clauses and early returns over nesting. Deep nesting means a missing extraction.
4. **Size is a signal.** A function past about 50 lines or a file past about 300 is probably doing two things. Generated code, kernels, and tables are exempt.
5. **No comments by default.** Code explains itself through names, types, and structure. Before writing a comment, change the code so the comment is not needed: rename, extract a function, or name a constant. A comment is written only when a competent reader could not recover the reason from the code itself: a non-obvious invariant, a workaround for a named bug or quirk, a reference to a spec or paper, a recorded trade-off, or an override of a default under the Precedence rule. It states why, never what, in as few lines as possible. A `TODO` carries an issue link.
6. **Docstrings state the contract.** Public API documents behavior, conventions, and failure modes, and does not repeat the signature. Private code has no docstring unless 4.5 would allow a comment there.
7. **Log through the project's logger.** Raw printing belongs only in thin CLI layers.

## 5. Slop

These are the signatures of careless generated code. A diff containing any of them is not done.

- Comments that narrate the work, restate the code, or address the reader (`// now we loop`, `// updated as requested`), and any comment that a better name or structure would have made unnecessary.
- `try`/`except`, null checks, or fallbacks around calls that cannot fail, or that hide a failure that should surface.
- An abstraction with one implementation, a wrapper that only forwards, a parameter or option nobody uses.
- Compatibility shims, aliases, or re-exports for callers that do not exist.
- A second helper that duplicates an existing one, or copy-pasted blocks that differ by a constant.
- Stubs, placeholder returns, or `NotImplemented` paths presented as finished work.
- Tests that assert on their own mocks, mirror the implementation line by line, or cannot fail.
- Reformatting, reordering, or renaming in lines the task did not need to touch.
- Docs with filler, hedging, marketing language, emojis, or a description of the change instead of the system.
- Claims of success without the command output that proves them.

## 6. Tests

1. Every behavior change ships with a test that fails without it.
2. Tests assert behavior and contracts through the public interface, not implementation details.
3. Tests are fast, isolated, and deterministic: in-memory fixtures, no network, no large files, unless explicitly marked otherwise.
4. Test layout mirrors code layout. Each test checks one behavior and is named for it.
5. Cover the edges: empty, boundary, invalid input, and each failure mode the contract names.

## 7. Repository structure

- **One obvious home for importable code,** which a newcomer finds in seconds.
- **Entry points are thin.** Scripts, CLIs, and `main` files parse inputs and call library code. They hold no logic worth testing.
- **The root holds metadata and entry points,** never logic or scratch files.
- **Generated artifacts stay out of version control,** in documented, ignored directories: weights, checkpoints, logs, datasets, build output, caches.
- **Structure follows what exists.** No directories, layers, or configuration for hypothetical needs. No empty placeholders.
- **Layout serves the consumer.** A library optimizes for a clear public API and stable import paths. A tool, for thin entry points, declarative configuration, and one-command reproducibility. A simulation, for versioned assets with provenance and seeded construction. Research, for a config per experiment and results traceable to a commit, a config, a seed, and a command.
- **Existing repo:** follow its layout and do not restructure unasked. **New code:** goes where a maintainer would look first, next to its most similar neighbors.

## 8. Documentation

1. Docs describe the system as it is now, in present tense. History lives only in the changelog and in decision records.
2. Each doc serves one reader and one purpose. A tutorial is not a reference, and a reference is not an explanation.
3. Plain statements only. Commands in fenced blocks, identifiers in backticks, a unit on every physical quantity, relative links, text diagrams.
4. **README:** what this is, who it is for, setup that works from a fresh clone, one verified minimal example, links onward, license. It does not restate configuration, formats, or layout.
5. **Developer docs** exist once they have real content: architecture (boundaries, dataflow, rejected alternatives), how to extend, and the domain conventions the code relies on.
6. **Decision records** capture choices that are non-obvious or costly to reverse: context, decision, alternatives, consequences. Accepted records are superseded, never edited.
7. **Docs change with the code** whenever a change touches a public signature, CLI flag, configuration key or default, environment variable, data or wire format, domain convention, directory layout, or dependency requirement.

## 9. Definition of done

Run the project's own commands. If none exist, say so and do not guess.

1. Format and lint: zero issues.
2. Type check, where the language or project has one: zero errors.
3. Tests: all green, including the new ones.
4. Docs: every trigger in 8.7 present in the diff has its docs change, and the README example still runs.
5. Diff: read `git status` and `git diff` line by line. Every changed line traces directly to the task; nothing from section 5, nothing out of scope, no secrets.

Resolve every warning you introduced.

## 10. Report

A few plain lines: what changed and where, what was verified and how, what was not verified and why, and what needs a human decision. No preamble, no step-by-step recap, no self-congratulation.
