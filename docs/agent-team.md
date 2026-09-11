# Project Pulse Agent Team

This document summarizes the custom agent team defined in `.github/agents/` that will build Mona's **Project Pulse** dashboard using GitHub Copilot CLI.

## Team roster

| Agent | Model | Definition file | Responsibility |
| --- | --- | --- | --- |
| Orchestrator | Claude Opus 4.7 (copilot) | `.github/agents/orchestrator.agent.md` | Breaks the Project Pulse request into tasks, delegates to Planner, Designer, and Coder, tracks progress, and reports the final result. It coordinates but does not implement. |
| Planner | Claude Opus 4.7 (copilot) | `.github/agents/planner.agent.md` | Researches the repository and brief, then writes an implementation plan with phases, file ownership, dependencies, parallel work, and validation. It plans but does not write code. |
| Coder | GPT-5.5 (copilot) | `.github/agents/coder.agent.md` | Implements the static app files (`app/index.html`, `app/styles.css`, `app/project-data.json`) and support configuration such as `.vscode/launch.json`, with clear structure and testable behavior. |
| Designer | Gemini 3.1 Pro (copilot) | `.github/agents/designer.agent.md` | Owns UI/UX, accessibility, information architecture, and visual design so the dashboard is polished and readable, not a plain page. |

## Model assignments

- **Orchestrator** — Claude Opus 4.7
- **Planner** — Claude Opus 4.7
- **Coder** — GPT-5.5
- **Designer** — Gemini 3.1 Pro

## How the team works together

1. The **Orchestrator** receives the Project Pulse request and asks the **Planner** for an implementation plan (`docs/project-pulse-plan.md`).
2. The Planner defines phases, assigns files, marks dependencies, and decides which work can run in parallel.
3. The Orchestrator delegates visual and accessibility decisions to the **Designer** and implementation to the **Coder**. Design guidance for the card layout, status badges, and hierarchy can be produced while the Coder prepares the data file.
4. The Coder builds `app/index.html`, `app/styles.css`, `app/project-data.json`, and `.vscode/launch.json` following the Designer's guidance.
5. The Orchestrator validates the result against the plan, runs the **Run Project Pulse Dashboard** launch configuration, and writes the final handoff.
