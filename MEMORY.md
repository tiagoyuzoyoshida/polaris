# MEMORY.md: Agent Memory Catalog

Index of `polaris/memory/`. Agents read this catalog first, then open only the notes whose scope matches the current task.

Maintained by the aide (under planner direction) at cycle close. The `polaris/memory/` directory starts empty and is created on first write; notes may be organized into nested subdirectories (for example, `memory/subsystems/`, `memory/conventions/`, `memory/gotchas/`).

**Rules:**
- **High bar:** Record only what is non-obvious, cross-cutting, or costly to rediscover from code (subsystem contracts and boundaries, subtle invariants, gotchas, external API quirks, non-obvious test/build setup). Never duplicate code or mirror obvious file trees.
- **Scoped freshness:** Every entry lists the code paths or topic it covers. At cycle close, check only the notes whose scope overlaps the cycle's changed files; update or delete any note that is stale.

| Note | Scope (paths / topic) | Summary | Last verified |
|---|---|---|---|
