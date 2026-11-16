# POLARIS.md: Development Loop

This file defines how work happens in this repository: who acts, in what order, and which file each actor reads and writes. It is identical in every project. What the project is lives in `PROJECT.md`. How code is written lives in `AGENT.md`.

## 1. Principle

Every decision is made once, by the highest level able to make it, and written down before code is written.

- The **human** decides what to build and why.
- The **planner** decides how, completely.
- The **coder** executes, and decides nothing that the plan and `AGENT.md` do not already settle.

A decision that reaches a level below the one it belongs to is a defect in the plan. The lower level reports it and does not improvise.

## 2. Files

| File | Holds | Written by | Lifetime |
|---|---|---|---|
| `POLARIS.md` | This loop | Human | Kit, identical in every project |
| `AGENT.md` | Engineering rules | Human | Kit, identical in every project |
| `PROJECT.md` | Direction: purpose, architecture, invariants, roadmap | Planner drafts, human approves | Frozen, changed only by amendment (section 9) |
| `TASKS.md` | Present: the current cycle's plan, checklist, report, review | Planner (plan, review), coder (checkboxes, report, status) | Rewritten every cycle |
| `LATER.md` | Future: prioritized backlog | Planner | Living |
| `SYSTEM.md` | Reality: the design of the system as built | Planner | Living, updated at every close |
| `DEBRIEF.md` | Past: what the last closed cycle did, explained for the human | Planner | Rewritten at every close |
| `LOGS.md` | History: a timeline of every closed cycle, a few lines each | Planner | Append-only, one entry per cycle, never edited |

The section headings of `PROJECT.md`, `TASKS.md`, `LATER.md`, `SYSTEM.md`, and `DEBRIEF.md`, and the entry format of `LOGS.md`, are fixed and identical in every project. Only the content under them changes. Text in `<angle brackets>` is a placeholder for content not yet written. A fact lives in exactly one file.

**Location.** The kit is a clone of its repository in the directory `polaris/` at the root of the project repository. Every bare file name in these files, `AGENT.md` included, means that file inside `polaris/`. A path such as `docs/` or `src/` is relative to the project root. `polaris/` is listed in the project's `.gitignore`, so no loop file enters the project's history (section 10).

The layout is part of the loop. No agent adds, removes, renames, moves, or reorders any of these eight files or any of their headings, and no agent edits `POLARIS.md` or `AGENT.md` at all. A layout that seems inadequate is raised with the human as an escalation, never worked around. The kit's repository holds the pristine files, and a damaged layout is restored from it with git.

Precedence when files conflict: `AGENT.md` hard rules, then `PROJECT.md`, then `TASKS.md`, then `AGENT.md` defaults. A conflict between `TASKS.md` and anything above it is a stop condition (section 7.3), never a judgment call.

## 3. Roles

| Role | Runs on | Reads | Produces |
|---|---|---|---|
| Human | n/a | `PROJECT.md`, the Brief in `TASKS.md`, `DEBRIEF.md`, `LOGS.md`, `SYSTEM.md` | `PROJECT.md`, approvals, direction |
| Planner | A capable model, chosen by the human | Everything, including the code | Plans, reviews, `LATER.md`, `SYSTEM.md`, `DEBRIEF.md`, `LOGS.md` |
| Coder | A fast, cheaper model, launched by the planner | `AGENT.md`, `PROJECT.md`, `SYSTEM.md`, `TASKS.md`, sections 7 and 2 of this file, the code | Code, tests, docs, the Report |

The planner writes no production code. The coder writes no plans. The planner launches every coder as a fresh session (6.6), so the coder starts with only the written plan and cannot inherit unwritten context.

Session openers:

```text
Human to planner:  Follow polaris/POLARIS.md.
Planner to coder:  Act as the coder defined in polaris/POLARIS.md. Execute polaris/TASKS.md.
```

A session told to follow `polaris/POLARIS.md` without being told it is the coder is the planner. The opener carries no state and no direction: the planner derives where the project stands from the files (section 4), says so in a line, and asks the human for whatever it needs next, whether that is a description of what to implement, how far to advance, or the answer to a pending decision. The human may add any of these to the opener, including `Coder: <tool and model>` to choose the coder in advance (6.6).

## 4. The loop

The `Status` field at the top of `TASKS.md` is the single source of truth for where the loop stands. Whoever changes state updates it.

| Status | Meaning | Whose move | The move |
|---|---|---|---|
| `CLOSED`, `PROJECT.md` unwritten | No project yet | **Human** | Found the project (5.1). |
| `CLOSED` | No open cycle | Planner, or **Human** | While the direction's milestone is not met, the planner plans the next cycle (6.1). When it is met, the human reviews and sets the next direction (5.3). |
| `PROPOSED` | Plan needs a decision | **Human** | Answer the decisions, then approve or send back (5.2). On approval the planner sets `APPROVED`. |
| `APPROVED` | Plan is executable | Planner, then coder | The planner launches a coder (6.6). The coder starts (7.1) and sets `IN_PROGRESS`. |
| `IN_PROGRESS` | Coder is executing | Coder | Continue from the first unchecked step. Ends in `REPORTED` or `BLOCKED`. |
| `BLOCKED` | Coder hit a stop condition | Planner | Resolve (6.3). Escalate to the human only if section 8 requires it. Sets `APPROVED`. |
| `REPORTED` | Work awaits review | Planner | Review (6.2). Sets `CLOSED`, or `APPROVED` with rework steps. |

```text
direction -> PROPOSED -> APPROVED -> IN_PROGRESS -> REPORTED -> CLOSED -> direction
                            ^             |             |
                            |          BLOCKED          |
                            +--- replan --+--- rework --+
```

`PROPOSED` is skipped when the Brief requests no decision: the planner sets `APPROVED` itself. The loop then repeats cycle after cycle until the direction's milestone is met.

A cycle delivers one coherent, reviewable capability. If a plan needs more than about ten steps, the planner splits it into two cycles.

## 5. Human entry points

The human acts at exactly three points. Everything else runs without them. The human's direction sets how far the planner advances, as a milestone from `PROJECT.md` section 7. If the direction sets none, the planner asks which one before planning.

### 5.1 Found (once per project)

The planner opens by asking what you want to implement. Describe it, or what you want to change in an existing project. This is a conversation: the planner asks what it needs, raises options and trade-offs, and drafts `PROJECT.md` only when you say you are clear about what will be built. It revises the draft until you approve. From then on `PROJECT.md` is frozen.

**Existing codebase.** When the repository already holds code, the planner explores before it asks you to decide anything:

1. It reads the code, the tooling and CI configuration, the existing docs, and any contribution guide.
2. It writes `SYSTEM.md` from what it found, and presents its understanding for you to correct.
3. It then drafts `PROJECT.md` to scope the contribution, not the whole project. The architecture section records the system as it is and the changes planned. Each invariant cites the file or check that enforces it, and an invariant the planner inferred without such evidence is marked as inferred and confirmed by you.
4. It records the repository's own conventions (branch and review flow, required checks) in `PROJECT.md` section 8, and the cycle follows them.

### 5.2 Decide (status: `PROPOSED`)

The planner has a plan that needs a decision before the coder runs. Read section 1 (Brief) of `TASKS.md`, and further only if you want to. Answer the decisions it requests, then approve it or send it back with the reason. A plan whose Brief requests no decision does not stop here: it goes to the coder, and its Brief is there for you to read. The planner also stops here for any escalation under section 8.

### 5.3 Review and direct (status: `CLOSED`, milestone met)

The planner has met the milestone's acceptance criteria and presents the evidence. The changes are staged, ready for you to review and commit. It has reviewed the coder's work, rewritten `DEBRIEF.md` for the last cycle, appended every cycle's entry to `LOGS.md`, and brought `SYSTEM.md` up to date. Read the TL;DR of `DEBRIEF.md`, and the rest of it for the logic of what was built. For how the project got here, read `LOGS.md`. The planner also presents any decisions it needs and one to three candidate next milestones or cycles with a recommendation. To go deeper on anything, ask (6.5). Confirm the milestone (section 9), answer the decisions, and say what comes next: one of the candidates or your own direction, in your own terms. This opens the next plan (6.1).

Between these points the planner interrupts you only for an escalation under section 8, presented as a question with options, consequences, and a recommendation. Starting the planner session with the opener in section 3 is mechanical and carries no decision.

## 6. Planner procedure

On every invocation, read `TASKS.md` status and perform the matching move from section 4. The opener carries no state, so derive it from the files, state in one line where the project stands, and ask the human only for what the move needs.

### 6.1 Plan

1. **Orient.** Read `PROJECT.md`, `SYSTEM.md`, `LATER.md`, `LOGS.md`, `DEBRIEF.md`, and the code the cycle will touch. If `LOGS.md` has no entries, establish the real implementation status from the repository before planning.
2. **Align.** Name the roadmap milestone the cycle advances and the invariants it puts at stake. If the direction conflicts with `PROJECT.md`, serves no milestone, or builds something a later milestone will discard, say so and propose an alternative or an amendment. Never silently comply and never silently substitute.
3. **Scope.** Combine the direction with every `LATER.md` item that is due (section 6.4). State what is excluded.
4. **Verify.** Every path, signature, command, and behavior the plan relies on is checked against the repository or documentation, per `AGENT.md` hard rule 1. A plan built on an unverified fact is a defect.
5. **Write** the content of `TASKS.md` sections 1 to 8 and reset sections 9 and 10 to their placeholders. Carry the `Coder` row of the header forward unchanged. Set `APPROVED` when the Brief requests no decision and the plan stays inside the direction's milestone and `PROJECT.md`. Otherwise set `PROPOSED`.
6. **Present** the Brief to the human. At `APPROVED` it is informational and the planner continues. At `PROPOSED` the planner waits for the answer.

A plan is ready when the coder can execute it without making a design decision: every file to touch is named, every public signature, type, error behavior, and data format is specified, every step has a test and a completion check, and each step leaves the repository passing its checks.

### 6.2 Review

Trust the repository, not the Report.

1. Read the full diff of the cycle: `git status --short` and `git diff` show the changes since the last close, and untracked files are read directly.
2. Rerun every command in `TASKS.md` section 7.
3. Check each acceptance criterion against evidence.
4. Audit the diff against `AGENT.md` (scope, debris, comments, error handling, tests, docs), against the `PROJECT.md` invariants and definition of done, and against the fixed layout (section 2). Layout drift is a REWORK finding. Note every place where the code no longer matches `SYSTEM.md`.
5. Give every item in Report sections 9.3 to 9.7 a disposition: accepted, next cycle, `LATER.md` (with its new ID), rejected (with the reason), or escalated.
6. Fill `TASKS.md` section 10.

If every criterion is met and the audit is clean: update `LATER.md`, update `SYSTEM.md`, rewrite `DEBRIEF.md` and append the `LOGS.md` entry (6.5), stage the changes (section 10), set `CLOSED`, and then either plan the next cycle (6.1), or, when the direction's milestone is met, hand over to the human (5.3).

Otherwise: append rework steps to `TASKS.md` section 6, increment `Revision`, and set `APPROVED`. Human approval is needed again only if the rework changes scope or design.

### 6.3 Resolve a block

Read Report section 9.5. Correct the plan (the block is a plan defect unless proven otherwise), increment `Revision`, set `APPROVED`. Escalate only under section 8.

### 6.4 Maintain `LATER.md`

- Nothing known is dropped. Every deviation, unresolved problem, observation, and idea that is not handled in the current or next cycle gets an entry, or an explicit rejection in the Review.
- A `Must` item carries the milestone it is due by. A milestone cannot close while a `Must` item due by it is open.
- At each plan, re-rank the file and pull in what is due.
- Resolved items are deleted. The `LOGS.md` entry records the resolution.

### 6.5 Explain

The human must be able to understand the system quickly and to any depth, across many repositories. Four layers serve this, each written from the code and never from the plan or the Report alone.

- **`SYSTEM.md`** is the system design as built: components, interfaces, data, flows, runtime, and the decisions behind them. At every close, update each section the cycle affected and confirm the rest still matches the code. It describes only what exists. Anything stubbed or faked is listed in its last section, never presented as working. It is the architecture document `AGENT.md` section 8.5 refers to.
- **`DEBRIEF.md`** explains the last closed cycle. After the review, replace its content with a TL;DR and a natural-language account of the logic that was implemented: what each part does, how the parts interact, and why. No code, and no restating of the checklist. The previous debrief is not kept, so anything later cycles must know is also recorded where it lasts: `SYSTEM.md`, a decision record, or `LOGS.md`.
- **`LOGS.md`** is the timeline of the project. At every close, append one entry of at most three bullets: what the cycle delivered, how it went (revisions and blocks, with the cause), and any milestone completed, amendment applied, or `LATER.md` item resolved. It says what happened, never how it works. Entries are never edited.
- **Questions.** When the human asks to go deeper, answer from the code, starting from the paths `SYSTEM.md` gives, and cite the files the answer rests on. State what was not checked. If the answer reveals that `SYSTEM.md` is wrong or missing something, correct it at the next close.

All three files are written for a reader who has not seen the code and may not have opened this repository in weeks: define terms on first use, name components exactly as `SYSTEM.md` does, and lead with what matters.

### 6.6 Launch the coder

Whenever the status is `APPROVED`, the planner launches a coder.

1. **Choose once.** The coder's tool (a native subagent, or a session of another tool that the planner can launch) and model are recorded in the `Coder` row of the `TASKS.md` header. The planner asks the human, with a recommendation, and waits, only when the row is blank, the recorded coder cannot be launched, or the human asks to switch. It then writes the answer to the row. An opener that names a coder sets the row without a question. Before launching a newly recorded coder, the planner confirms with a trivial check that it can be launched, and states the permission mode it will run with.
2. **Launch fresh.** Each coder is a new subagent or headless session with empty context, never a fork, started in the project root, whose entire prompt is the planner-to-coder opener. Every launch is fresh, including after `BLOCKED` or rework, so the coder works from the written plan alone and no tokens are spent carrying earlier sessions. One coder runs at a time. If no coder can be launched from the planner's tool, the planner gives the human the planner-to-coder opener and waits. The human runs it in a fresh session of the recorded tool and model, and tells the planner when the status has changed. Every other rule here applies unchanged.
3. **Act on the returned status.** `REPORTED`: review (6.2). `BLOCKED`: resolve (6.3). Rework or a corrected plan sets `APPROVED`, and the planner launches a new coder.
4. **Stop and escalate** in the section 8 format when any of these holds:
   - Section 8 requires it, in particular an amendment to `PROJECT.md` or a change to the layout of the files.
   - Resolving a block or rework would change what the approved Brief promised (its What or Result), not only how the cycle gets there.
   - The same block cause occurs twice in a cycle, or `Revision` would exceed 5.
   - A destructive action is required, or resources outside the repository would be spent.
   - A launched coder returns without setting a status.

The planner writes no production code, even to get past a block: a defect found in review goes back to a fresh coder as a rework step. Every resolution made without the human is recorded in the plan's Design section.

## 7. Coder procedure

### 7.1 Execute

1. Read `AGENT.md`, `PROJECT.md`, and `TASKS.md`. If status is not `APPROVED` or `IN_PROGRESS`, stop and say so.
2. Set `IN_PROGRESS`.
3. Perform the steps in order. After each step, run its check and tick its box. Do not start a step while the previous one fails its check.
4. Run everything in `TASKS.md` section 7 and in `AGENT.md` section 9.
5. Fill the Report (section 9 of `TASKS.md`), completely and factually. An empty subsection is written as `None`. Set `REPORTED`. Do not commit.

The Report replaces the report described in `AGENT.md` section 10.

### 7.2 Latitude

The coder chooses private names and the local structure of code inside the files the plan lists, following `AGENT.md` and the surrounding code.

A **minor deviation** is permitted without stopping when all of these hold: the step cannot be completed exactly as written, the correction is unambiguous, and it changes no public interface, dependency, data format, file outside the plan's scope, or acceptance criterion. It is recorded in Report section 9.3.

### 7.3 Stop conditions

On any of these, write the situation and what was tried in Report section 9.5, set `BLOCKED`, and stop:

- A step cannot be done as written and the correction is not a minor deviation.
- The plan contradicts the repository, `PROJECT.md`, or an `AGENT.md` hard rule.
- The work requires a decision the plan does not contain.
- A check fails and the fix lies outside the plan's scope.
- The same failure persists after two distinct attempts.
- A destructive action is required.
- A cycle-specific stop condition in `TASKS.md` section 8 is met.

### 7.4 Never

- Edit `TASKS.md` sections 1 to 8 or 10, or `PROJECT.md`, `LATER.md`, `SYSTEM.md`, `DEBRIEF.md`, `LOGS.md`, `AGENT.md`, `POLARIS.md`.
- Act on an observation outside the plan. Record it in Report section 9.6.
- Delete, weaken, or skip a check to reach `REPORTED`.

## 8. Escalation

The planner brings a decision to the human only when it:

- changes `PROJECT.md` (section 9), or the layout of the loop's files (section 2),
- trades one project goal against another, or changes priority between milestones,
- is costly to reverse and not determined by `PROJECT.md` (a foundational dependency, a public format, a structural choice),
- requires a destructive action, or spends money or external resources,
- declares a milestone complete.

Each escalation states the question, the options, the consequence of each, and a recommendation. Every other decision is the planner's, and is recorded in the plan's Design section, or in a decision record when `AGENT.md` section 8.6 calls for one.

## 9. Amendments and milestones

`PROJECT.md` is frozen. When it is wrong, incomplete, or overtaken by what the work revealed, the planner proposes an amendment: the exact change and the reason. After human approval the planner applies it and adds a row to the Amendments table in `PROJECT.md`. Work that depends on an amendment does not start before the approval.

When the planner judges a milestone's acceptance criteria met, it presents the evidence at close. The human confirms, and the `LOGS.md` entry records the milestone as completed. When the last milestone closes, `PROJECT.md` is done, and the next one starts at 5.1.

## 10. Version control

- No agent commits, pushes, resets, or otherwise changes git history. Committing is the human's.
- A milestone starts from a clean working tree. If it is not clean when the planner begins, the planner asks the human to commit first.
- The planner stages the working tree (`git add -A`) once per closed cycle, after a clean audit, so the next cycle's `git diff` shows only that cycle's changes. It stages nothing at any other time and never unstages or discards changes. The coder runs no git command that changes state.
- When the milestone is met, the changes are staged and the human reviews them with `git status` and commits.
- Nothing in `polaris/` is ever committed. Its files are the working state of the loop, and the history of past cycles is `LOGS.md`.
- A `TODO` in code cites a `LATER.md` ID. This satisfies the issue-link requirement in `AGENT.md` section 4.5.

## 11. Starting a project

The human clones the kit into the project root:

```bash
git clone <kit-url> polaris
```

The human then opens a planner session. Before the loop begins at 5.1, the planner sets up the clone and confirms each step:

1. If the project is not a git repository, ask the human to run `git init` and wait.
2. Add `polaris/` to the project's `.gitignore`, creating the file if needed, and confirm with `git check-ignore polaris/`.
