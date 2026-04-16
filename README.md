# How — a skill for explaining codebases

A plugin for Cursor and Claude Code that adds the `/how` skill: ask how a subsystem, feature, or flow works, and the agent produces a clear architectural explanation at the level of a senior engineer onboarding onto a new area.

## What it does

Two modes, picked automatically from your question:

- **Explain** (default) — explore the codebase and produce a structured explanation with sections for Overview, Key Concepts, How It Works, Where Things Live, and Gotchas.
- **Critique** — explain first, then spawn multiple independent critics to surface architectural problems.

For complex questions that span multiple files or services, the skill decomposes the question into 2–4 parallel exploration angles and spawns explorer subagents that gather findings in parallel. A synthesis agent then reconciles their findings into a single coherent explanation. Simple questions skip the fan-out and run end-to-end in a single agent.

## Example prompts

- "How does message virtualization work?"
- "Walk me through what happens when a user sends a message"
- "How is the auth service structured? Also critique the design."
- "How does the auth middleware check permissions?"

## Structure

```text
how/
├── .cursor-plugin/
│   └── plugin.json          # Cursor manifest
├── .claude-plugin/
│   ├── plugin.json          # Claude Code manifest
│   └── marketplace.json     # Marketplace metadata
└── skills/
    └── how/
        ├── SKILL.md
        └── references/
            ├── explainer-prompt.md
            ├── explorer-prompt.md
            ├── critic-prompt.md
            └── critique-rubric.md
```

`SKILL.md` contains the top-level routing logic (simple vs. complex, explain vs. critique). The `references/` folder holds the prompt templates that the agent hands to each subagent, plus the critique rubric used during the critique pass.

## Installation

### Cursor

Install as a Cursor plugin — the `.cursor-plugin/` manifest is picked up automatically.

### Claude Code

Install the plugin directory locally. The `.claude-plugin/` manifest enables auto-discovery of the `how` skill.

## License

MIT
