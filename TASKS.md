# TASKS.md: Cycle <N>, <title>

| | |
|---|---|
| Status | CLOSED |
| Milestone | <ID and name from PROJECT.md section 7> |
| Planned | <YYYY-MM-DD> |
| Revision | 1 |
| Coder | <tool, model, and thinking level, set at the first launch> |
| Aide | <tool, model, and thinking level, set at the first launch> |

## 1. Brief

<For the human. At most ten lines. No implementation detail.>

- **What:** <the capability this cycle delivers>
- **Why now:** <what it unlocks, and why before the alternatives>
- **Result:** <what will be observably true when it closes>
- **Decisions requested:** <questions with options and a recommendation, or `None`>

## 2. Acceptance criteria

<Each is checkable by a command or a concrete observation.>

- **A1.** <criterion>

## 3. Context

- **Advances:** <milestone, and which part of its acceptance>
- **Invariants at stake:** <IDs from PROJECT.md section 5, and how the plan protects each>
- **LATER items addressed:** <IDs, or `None`>
- **Memory to read:** <paths under `polaris/memory/` relevant to this cycle, or `None`>
- **Existing code to follow:** <paths of the nearest similar code and its tests>

## 4. Scope

**Create or modify:**

- `<path>`: <purpose>

**Excluded:** <adjacent work deliberately left out, with the LATER ID where it is tracked>

## 5. Design

<Every decision the coder would otherwise have to make: public signatures and types, data formats, error behavior, dataflow, dependency direction. Rejected alternatives with one line of reason each.>

## 6. Steps

<Ordered. Each step leaves the repository passing its checks.>

- [ ] **S1. <title>**
  - Files: `<path>`
  - Do: <exact change>
  - Test: <test to add or update, and what it asserts>
  - Done when: <command and expected result>

## 7. Verification

<The commands from PROJECT.md section 8, plus cycle-specific checks, each with its expected result.>

```bash
<command>
```

## 8. Stop conditions

<Cycle-specific conditions, in addition to POLARIS.md section 7.3, or `None`.>

---

## 9. Report

<Written by the coder. Facts only. An empty subsection is written as `None`.>

### 9.1 Implemented

<What was built and how, by step.>

### 9.2 Verification results

| Check | Command | Result |
|---|---|---|

### 9.3 Deviations from the plan

<What differs, where, and why.>

### 9.4 Problems encountered and fixed

<Problem, cause, fix.>

### 9.5 Blocked or unresolved

<What could not be solved or was not the coder's to decide, what was tried, and what is needed.>

### 9.6 Observations

<Adjacent problems, improvements, optimizations, and ideas noticed and left untouched.>

### 9.7 Proposed next steps

<Candidates with reasoning tied to the PROJECT.md roadmap.>

---

## 10. Review

<Written by the planner.>

**Verdict:** <ACCEPTED or REWORK>

| Criterion | Met | Evidence |
|---|---|---|

**Findings:** <audit results against AGENT.md and PROJECT.md, or `None`>

| Report item | Disposition |
|---|---|
| <9.x item> | <accepted, next cycle, LATER L-nnn, rejected: reason, or escalated> |
