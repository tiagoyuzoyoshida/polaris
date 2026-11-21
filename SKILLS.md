# SKILLS.md: Agent Skill Catalog

Index of `polaris/skills/`. Agents read this catalog first, then open only the skills whose trigger matches the current task.

Kit file, identical across projects. Agents never edit `SKILLS.md` or `polaris/skills/`. Add new domain skills over time by creating a `.md` file in `polaris/skills/` and adding a row under **Domain skills** — `POLARIS.md` does not need to change.

## Workflow skills

Built into the Polaris loop and triggered at fixed workflow points or explicit human commands.

| Skill | When to use | Roles |
|---|---|---|
| `skills/writing.md` | Writing or updating any human-facing file (`README.md`, `SYSTEM.md`, `DEBRIEF.md`, `LOGS.md`, user docs) | Aide, Coder, Planner |
| `skills/diagrams.md` | Creating or updating Mermaid diagrams and visual explanations (`SYSTEM.md`, docs, deep-dive answers) | Aide, Coder, Planner |
| `skills/update-polaris.md` | Only when the human explicitly asks to update `polaris/` from GitHub | Planner |

## Domain skills

Reusable engineering and craft skills loaded on demand. During planning (`POLARIS.md` 6.1), the planner selects any skill whose trigger matches the cycle and lists it in `TASKS.md` section 3 (`Skills to read`).

| Skill | When to use | Roles |
|---|---|---|
