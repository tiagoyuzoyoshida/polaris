# Polaris

An agent workflow for automating implementation with AI coding agents.

## Benefits

- **Intelligence where it is needed.** A capable model plans and reviews, a fast model executes code, and a low-thinking aide handles read-only exploration, deep-dive questions, and documentation updates.
- **Low-context by design.** Agents navigate codebase knowledge through a compact memory catalog and open only the notes relevant to the current cycle, keeping context lean and quality high.
- **You understand what the agents are doing.** Every decision is written down before code exists, a visual Mermaid overview shows how the system works, each finished cycle comes with a short debrief, and a timeline records how the project got here.
- **The project stays reliable.** Every plan is checked against a project contract you approved, and the planner reviews the repository itself, not the coder's report.
- **Autonomy up to a milestone.** You choose how far the planner goes, as a milestone of your roadmap. It runs on its own up to that point and stops only for decisions that are yours.

## Use it

Clone it into the root of the project you are working on:

```bash
cd your-project
git clone https://github.com/tiagoyuzoyoshida/polaris.git
```

Open a session in your agent tool (a CLI, a desktop app, or an IDE) at the project root, using the model you trust most for planning and review, and send:

```text
Follow polaris/POLARIS.md.
```

Send the same message at the start of every session. The planner reads the state of the project and picks up where it left off.

**Part 1: define the project, once.** The planner sets up the clone: it adds `polaris/` to your project's `.gitignore`. It then asks what you want to implement, and you discuss it until the picture is clear. If the project already has code, the planner explores it first and writes `SYSTEM.md` from what it finds. It then writes `polaris/PROJECT.md`: purpose, architecture, invariants, and a roadmap of milestones, scoped to your contribution when the code already exists. You approve it, and from then on it changes only by an amendment you approve.

**Part 2: iterate.** Work proceeds in cycles, each delivering one reviewable capability. You choose how far to go, as a milestone of your roadmap. The planner plans a cycle, launches a fresh coder, reviews the result against the repository, writes a debrief, and repeats until the milestone is met. It stops and asks you only where a decision is yours:

- a plan whose brief asks you to choose between options,
- anything that would change the project's direction, such as an amendment to `PROJECT.md`,
- the end of the milestone, where you review the result and say what comes next.

The planner asks which tool, model, and thinking level to use for the **coder** and **aide** the first time each is needed, and it can call a tool other than its own, for example Claude Code launching a coder in Antigravity, or the reverse.

## The files

| File | Purpose |
|---|---|
| `POLARIS.md` | The rules of the workflow: who does what, in which order, and when the planner must stop and ask you. |
| `AGENT.md` | The engineering standards every coder follows when writing code: design, tests, comments, and what counts as done. |
| `SKILLS.md` | Agent skill catalog indexing `polaris/skills/`: workflow skills (writing, diagrams, updating Polaris) and reusable domain skills loaded only when relevant. |
| `PROJECT.md` | The agreement you approve at the start: what the project is for, its architecture, the rules every change must respect, and the roadmap of milestones. |
| `TASKS.md` | The cycle in progress: the plan the coder follows, its report, and the planner's review. Its status says whose move it is. |
| `LATER.md` | The backlog: problems, ideas and open questions found along the way that are not part of the current cycle, in priority order. |
| `MEMORY.md` | Agent memory catalog indexing `polaris/memory/` (created on first write): non-obvious conventions, contracts, and gotchas loaded only when relevant. |
| `SYSTEM.md` | Visual, plain-language overview of how the system works today (with Mermaid diagrams). Read this to understand the project at a glance. |
| `DEBRIEF.md` | What the last cycle did and why, in plain language, with a TL;DR first. Read this after a cycle. |
| `LOGS.md` | A short timeline of every cycle: what each delivered and how smoothly it went. Read this to see how the project progressed. |

`POLARIS.md`, `AGENT.md`, `SKILLS.md`, and `skills/` are Polaris itself. The other seven files (and `memory/` as it grows) are specific to your project, and the loop fills them in as it goes.

## License

MIT. See `LICENSE`.
