# Project Pulse Implementation Plan

Prepared by the Planner at the request of the Orchestrator.

## Goal

Build a small static **Project Pulse** dashboard that helps contributors quickly understand Mona's team projects: each project's name, owner, status, recent activity, and priority, rendered as polished cards and runnable from a VS Code launch configuration.

## Implementation phases

### Phase 1 — Design direction (Designer)
- Define the card-based layout, status badge colors, typography, spacing, and visual hierarchy.
- Define accessibility requirements: semantic markup, readable contrast, keyboard-friendly structure, `aria` labels where needed.
- Output: design notes that the Coder follows.

### Phase 2 — Data model (Coder)
- Create `app/project-data.json` with a top-level `projects` array.
- Each project includes `name`, `owner`, `status`, `recentActivity`, and `priority`.

### Phase 3 — Markup and styling (Coder, guided by Designer)
- Create `app/index.html` with the exact title "Project Pulse", link `styles.css`, fetch `project-data.json`, and render one `.project-card` per project inside a `.dashboard` container.
- Create `app/styles.css` with `.dashboard` and `.project-card` selectors, `border-radius`, `box-shadow`, status badges, and a responsive grid.

### Phase 4 — Run configuration (Coder)
- Create `.vscode/launch.json` (strict JSON) with a configuration named **Run Project Pulse Dashboard** that runs `python3 -m http.server 5500` from the `app` directory and uses `serverReadyAction` to open `http://localhost:%s/index.html`.

### Phase 5 — Validation and handoff (Orchestrator)
- Verify every file and write `docs/final-handoff.md`.

## File assignments

| File | Owner | Notes |
| --- | --- | --- |
| `app/project-data.json` | Coder | Source of truth for project cards |
| `app/index.html` | Coder (Designer reviews) | Title, structure, card rendering script |
| `app/styles.css` | Coder (Designer specifies) | Polished card UI, badges, responsive layout |
| `.vscode/launch.json` | Coder | Launch configuration for the preview server |
| `docs/final-handoff.md` | Orchestrator | Validation summary and handoff |

## Designer and Coder responsibilities

- **Designer**: visual language, accessibility, card hierarchy, badge semantics (status and priority), responsive breakpoints. Does not write the final files but reviews them.
- **Coder**: implements all files listed above exactly as specified, keeps JSON strict, and makes the dashboard runnable.

## Dependencies

- `app/index.html` depends on `app/project-data.json` (field names must match: `name`, `owner`, `status`, `recentActivity`, `priority`).
- `app/index.html` depends on `app/styles.css` class names (`.dashboard`, `.project-card`, badge classes).
- `.vscode/launch.json` depends on the `app/` directory existing and containing `index.html`.
- Validation depends on all four files being complete.

## Parallel work decisions

- Designer's design direction (Phase 1) and Coder's data model (Phase 2) can run in parallel — they do not depend on each other.
- `app/styles.css` and `app/index.html` are written together after Phase 1 and Phase 2 complete.
- `.vscode/launch.json` can be written in parallel with Phase 3 because it only depends on the `app/` folder path.
- Validation runs last and is not parallelized.

## Validation expectations

- `app/index.html` contains "Project Pulse", references `styles.css` and `project-data.json`, and renders `project-card` elements showing status, recentActivity, and priority.
- `app/styles.css` contains `.dashboard`, `.project-card`, `border-radius`, and `box-shadow`.
- `app/project-data.json` parses as JSON and has a `projects` key with all required fields on every project.
- `.vscode/launch.json` parses as JSON, is named "Run Project Pulse Dashboard", serves from `app`, and opens `index.html`.
- Running the launch configuration opens the dashboard (cards visible), not a directory listing.
