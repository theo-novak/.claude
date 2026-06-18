# ~/.claude setup

A few definitions on how I structure my root `.claude` environment so you can replicate it and work on projects the same way I do.

## What this repo is

Claude Code stores a lot of machine-local state under `~/.claude` — settings, plugins, session history, caches, and per-project transcripts. Most of that should stay on your machine.

This repo captures the **portable part**: custom subagent definitions in `agents/`. Clone or copy these into your own `~/.claude/agents/` to get the same domain specialists without syncing personal config or session data.

## Layout

```
~/.claude/
├── agents/          ← versioned here (the only tracked content)
├── settings.json    ← local (not in this repo)
├── plugins/         ← local
├── projects/        ← local session transcripts
└── …                ← everything else local
```

Only `agents/` (plus this README and `.gitignore`) is meant to be shared. The rest of `~/.claude` is intentionally ignored.

## Agents

Each file in `agents/` is a **subagent prompt** — a markdown file with YAML frontmatter that tells Claude Code when to delegate work and how that specialist should behave.

| Agent | Use for |
|-------|---------|
| `python-engineer` | Python scripts, packages, tests, tooling |
| `data-engineer` | DuckDB, pandas, financial data pipelines |
| `react-engineer` | React, Next.js, TypeScript UI |
| `html-engineer` | Static HTML/CSS/vanilla JS |
| `streamlit-engineer` | Streamlit apps and stlite dashboards |
| `csharp-engineer` | C#, Rhino/Grasshopper plugins |
| `cpp-engineer` | C++, CMake, performance-sensitive code |
| `rust-engineer` | Rust crates, async, FFI |
| `quant-researcher` | Quant finance research projects (portfolio layout, honest OOS eval) |

Frontmatter fields:

- **name** — subagent identifier
- **description** — when the orchestrator should route work here
- **model** — preferred model for that specialist
- **tools** — allowed tool set

The body is the system prompt: conventions, gotchas, and output expectations for that domain.

## Replicate this setup

1. Clone this repo (or copy `agents/`).
2. Place the `agents/` folder at `~/.claude/agents/` (create it if missing).
3. Keep your own `settings.json`, plugins, and project data local — do not commit them.

After that, Claude Code can dispatch to the same specialists when tasks match each agent's description.

## Why only agents?

Agents encode **how I want work done** — stack choices, review standards, project layouts. That travels well.

Settings, plugins, and session history are **environment-specific**. They change often, may contain paths or preferences tied to one machine, and do not help someone else work the same way. Versioning agents alone keeps the repo small and safe to share.
