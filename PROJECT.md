# PROJECT.md: <project name>

> The project's contract: what is being built, why, and the constraints every change must respect.
> Frozen. Changed only by amendment (`POLARIS.md` section 9).
> Engineering rules live in `AGENT.md` and the development loop in `POLARIS.md`. Neither is repeated here.

## 1. Purpose

<What this project is, who or what consumes it, and why it should exist. One or two paragraphs.>

## 2. Scope

**In scope:**

- <capability>

**Out of scope:**

- <what this project deliberately is not, including plausible features it will not have>

## 3. Success criteria

<The observable, measurable conditions under which this work is done.>

## 4. Architecture

### 4.1 Overview

<Text diagram of the components and what flows between them.>

### 4.2 Responsibilities

<What each component owns and what it must not do.>

### 4.3 Dependency boundaries

<Which components may depend on which. Stated as rules that a diff can be checked against.>

## 5. Invariants

<Properties that hold after every cycle. Numbered so plans and reviews can cite them.>

### I1. <name>

<The rule, with permitted and forbidden examples where a violation would be easy to miss.>

## 6. Contracts

<The interfaces, data models, and domain conventions that must stay stable: one subsection each. Omit the section's subsections that do not apply.>

## 7. Roadmap

<Strategy: the order of attack and why. Then the milestones, each a working, verified capability.>

| ID | Milestone | Acceptance |
|---|---|---|
| M1 | <capability> | <verifiable condition> |

## 8. Stack and commands

<Language, runtime, and key dependencies, then the exact commands the definition of done runs.>

| Check | Command |
|---|---|
| Format and lint | `<command>` |
| Type check | `<command>` |
| Tests | `<command>` |

## 9. Project definition of done

<Checks specific to this project, in addition to `AGENT.md` section 7.>

## 10. Amendments

| Date | Change | Reason |
|---|---|---|
