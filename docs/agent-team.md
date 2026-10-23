# Agent team

For Mona's Project Pulse dashboard, I will use a compact custom team defined under `.github/agents/` and orchestrated with GitHub Copilot CLI in a Codespace.

- Planner — Model: Claude Opus 5.5 (copilot) — researches the repo, dependencies, and edge cases, then produces a practical implementation plan for the dashboard work. Definition: `.github/agents/planner.agent.md`
- Designer — Model: Gemini 3.7 Flash (copilot) — focuses on dashboard UX, accessibility, interaction flow, visual hierarchy, and responsive styling. Definition: `.github/agents/designer.agent.md`
- Coder — Model: GPT-5.5 (copilot) — implements the app logic and code changes within the assigned file scope, including runnable app support when needed. Definition: `.github/agents/coder.agent.md`
- Orchestrator — Model: Claude Opus 5.5 (copilot) — coordinates the Planner, Designer, and Coder, breaks work into phases, manages file ownership, and verifies the integrated result. Definition: `.github/agents/orchestrator.agent.md`

This setup uses GitHub Copilot CLI in a Codespace to manage the workflow, delegate specialized tasks, and keep the dashboard build organized across planning, design, and implementation.
