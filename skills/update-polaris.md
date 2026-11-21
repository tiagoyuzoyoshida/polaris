# Skill: Update Polaris

Runbook for upgrading an existing `polaris/` directory inside a project to the latest version of Polaris from GitHub without losing project state.

**Trigger:** Run this skill **only** when the human explicitly asks to update Polaris. It is the sole human-authorized exception to the rule that agents do not replace kit files.

## File classification

Every Polaris installation contains three kinds of files:

| Category | Files | Action on update |
|---|---|---|
| **Kit files** | `POLARIS.md`, `AGENT.md`, `SKILLS.md`, `skills/`, `README.md`, `LICENSE` | Replace completely with the new clone's versions. |
| **Pure project data** | `memory/` (all files and subdirectories) | Copy verbatim from old `polaris/` into the new clone. |
| **Templated project files** | `PROJECT.md`, `TASKS.md`, `LATER.md`, `MEMORY.md`, `SYSTEM.md`, `DEBRIEF.md`, `LOGS.md` | Migrate all project content from old `polaris/` into the new clone's templates. |

## Procedure

### 1. Check for a newer version

From the project root:

1. Read the remote URL and current commit of `polaris/`:
   ```bash
   git -C polaris remote get-url origin
   git -C polaris rev-parse HEAD
   ```
2. Query the remote default branch head without modifying `polaris/`:
   ```bash
   git -C polaris ls-remote origin HEAD
   ```
3. If the local commit matches the remote `HEAD`, report that `polaris/` is already up to date and stop.

### 2. Clone fresh into a temporary sibling directory

Clone the remote into a fresh sibling directory (for example, `polaris-update/`) at the project root so the existing `polaris/` remains untouched until migration is verified:

```bash
git clone <remote-url> polaris-update
```

Summarize what changed between the old commit and the new clone (for example, `git -C polaris-update log --oneline <old-sha>..HEAD`, or diffing the kit files if commit history differs) so you know every template or rule change to account for during migration.

### 3. Transfer project data

1. **Copy `memory/` verbatim** (if `polaris/memory/` exists):
   Copy the entire `polaris/memory/` tree into `polaris-update/memory/`, preserving subdirectories and file contents exactly.
2. **Migrate each templated project file** (`PROJECT.md`, `TASKS.md`, `LATER.md`, `MEMORY.md`, `SYSTEM.md`, `DEBRIEF.md`, `LOGS.md`):
   - Compare the headings, header tables, and column structure of the old file against the new template in `polaris-update/`.
   - Preserve **100% of the project's actual content**:
     - `PROJECT.md`: all sections, invariants, roadmap milestones, and Amendments table.
     - `TASKS.md`: current `Status`, `Milestone`, `Planned`, `Revision`, `Coder`, and `Aide` rows, plus any active cycle content.
     - `LATER.md`: all open backlog items and IDs.
     - `MEMORY.md`: all catalog rows matching `memory/`.
     - `SYSTEM.md`, `DEBRIEF.md`, `LOGS.md`: all existing content and history entries verbatim.
   - Adopt any **new** headings, fields, or columns introduced by the new template:
     - Populate them from existing project facts if unambiguous (for example, setting a new `Skills to read` line to `None` when a cycle is `CLOSED`), or use the template's placeholder if not yet applicable.
     - If a template change restructures or removes a section in a way that could drop or alter project meaning, **stop and ask the human** before finalizing.
   - If the old `polaris/` had any unexpected extra files outside the table above, list them and ask the human whether to copy or discard them.

### 4. Verify before swapping

Before touching the old `polaris/`:

1. Confirm every templated project file in `polaris-update/` contains the migrated project data and that no `<placeholder>` accidentally overwrote real content.
2. Confirm `polaris-update/memory/` has the exact same files as `polaris/memory/` (when present).
3. Confirm `polaris-update/` has clean kit files matching the new clone (`POLARIS.md`, `AGENT.md`, `SKILLS.md`, `skills/`).
4. Confirm `polaris/` is still ignored by the project's git repository (`git check-ignore polaris/`).

### 5. Swap and clean up

Once verification passes:

1. Delete the old `polaris/` directory and rename `polaris-update/` to `polaris/`:
   ```bash
   rm -rf polaris && mv polaris-update polaris
   ```
2. Confirm `polaris/` is in place and ignored by git (`git check-ignore polaris/` and `git status --short`).

### 6. Report

Give the human a concise summary:
- Old commit → new commit.
- Key changes/new features in the updated Polaris kit.
- Any new template fields or headings added to project files during migration (and how they were populated).
