# Project Pulse dashboard implementation plan

## Summary and intended outcome

Build a lightweight, polished static **Project Pulse** dashboard for contributors. The first view must make it easy to identify active projects, each project’s owner, status, recent activity, priority or risk, and a short contributor-friendly description of the project’s state.

The delivered dashboard will consist of:

- `app/index.html` — accessible dashboard structure and deterministic rendering of project records.
- `app/styles.css` — responsive, polished visual system for the dashboard and project cards.
- `app/project-data.json` — the dashboard’s project data source.
- `.vscode/launch.json` — a runnable VS Code launch configuration named **Run Project Pulse Dashboard**.

### Scope assumptions

1. This is a small static application with no build system, framework, package manifest, backend, or authentication.
2. The browser must load `project-data.json` over HTTP rather than relying on `file://`; therefore the launch configuration must start `python3 -m http.server 5500` from `app/`.
3. `index.html` must link `styles.css`, request or otherwise load `project-data.json`, and render visible project cards from the `projects` data rather than hard-coding the cards alone.
4. The page title and visible primary heading must use the exact text **Project Pulse**.
5. `project-data.json` must have a top-level `projects` array. Every project must include non-empty `name`, `owner`, `status`, `recentActivity`, and `priority` values.
6. At least several representative project records should be supplied so the card layout, varied statuses, and varied priority/risk treatments are meaningfully visible.
7. No changes outside the four implementation files are required for the feature. The Orchestrator coordinates and verifies; it does not implement application changes.

## Team responsibilities

| Role | Responsibility | File ownership |
| --- | --- | --- |
| **Orchestrator** | Convert this plan into phases, issue scoped prompts, prevent overlapping edits, ensure dependencies are met, integrate completed work, and perform final cross-file verification. The Orchestrator must not make implementation edits. | No implementation-file ownership; read/review all four deliverables. |
| **Designer** | Define and implement the visual hierarchy, responsive layout, accessible presentation, card/badge treatment, spacing, contrast, and deterministic CSS hooks. Deliver a polished dashboard rather than a plain document. | `app/styles.css` only. |
| **Coder** | Create deterministic, testable markup and client-side behavior; create valid project data; create the runnable VS Code configuration; validate technical behavior and report failures clearly. | `app/project-data.json`, `.vscode/launch.json`, then `app/index.html`. |
| **Planner** | This plan was prepared from the repository’s agent definitions, Project Pulse brief, exercise instructions, and existing VS Code task configuration. | `docs/project-pulse-plan.md` only. |

The ownership split deliberately prevents simultaneous edits to a file. `app/index.html` is assigned only to Coder, and `app/styles.css` only to Designer.

## Shared implementation contract

Before implementation begins, the Orchestrator must communicate the following stable interface to Designer and Coder:

- Root layout hook: `.dashboard`
- One card per record: `.project-card`
- Each card exposes readable name, owner, status, recent activity, and priority/risk information.
- The page uses semantic landmarks and a clear heading hierarchy.
- Status and priority must be understandable in text; color must reinforce meaning, not be the only cue.
- The Coder will render cards using the agreed classes and data-field structure.
- The Designer will style these stable hooks without changing HTML or JavaScript behavior.
- The data source path is `project-data.json`, relative to `app/index.html`.

This is an explicit coordination artifact, not a request for a new shared file.

## Dependency-aware implementation phases

### Phase 0 — Orchestrator kickoff and contract confirmation

**Owner:** Orchestrator  
**Files modified:** none

1. Read this plan, `docs/agent-team.md`, the Project Pulse brief, and the relevant agent definitions.
2. Give Designer the visual/accessibility scope and give Coder the data, launch, and HTML scopes.
3. Confirm the shared implementation contract above before either specialist edits files.
4. Tell agents to remain within their assigned files and not stage, commit, or push.

**Dependencies:** None.  
**Completion condition:** Both specialists have the same card hooks, required data fields, launch requirements, and validation target.

### Phase 1A — Data contract and launch support

**Owner:** Coder  
**Files:** `app/project-data.json`, `.vscode/launch.json`

1. Create `app/project-data.json` as strict JSON.
2. Add a top-level `projects` array with multiple realistic dashboard records.
3. Ensure each record contains non-empty `name`, `owner`, `status`, `recentActivity`, and `priority` properties.
4. Create `.vscode/launch.json` as strict JSON with no comments.
5. Add a configuration named exactly **Run Project Pulse Dashboard**.
6. Configure it to run `python3 -m http.server 5500` with `cwd` set to `${workspaceFolder}/app`.
7. Add `serverReadyAction` so the browser opens `http://localhost:%s/index.html`, not the server directory root.

**Dependencies:** Phase 0 only.  
**Completion condition:** The data parses, the expected fields are present, and the launch definition has the required command, working directory, configuration name, and index URL.

### Phase 1B — Visual system and responsive styling

**Owner:** Designer  
**Files:** `app/styles.css`

1. Create the dashboard visual system using the agreed `.dashboard` and `.project-card` hooks.
2. Establish a readable information hierarchy for title, summary/context, project name, metadata, status, recent activity, and priority/risk.
3. Use a card-based layout with visible project boundaries, rounded corners, shadows, readable spacing, and clear typography.
4. Provide distinct, text-supported badge treatments for status and priority/risk.
5. Use responsive grid or flex behavior so cards remain readable at narrow and wide viewport sizes.
6. Support accessible contrast and avoid conveying status or priority with color alone.
7. Include sensible focus treatment for any interactive elements introduced by the HTML implementation.

**Dependencies:** Phase 0 only. It does not need final data values because it works against the agreed card hooks and field categories.  
**Completion condition:** `app/styles.css` contains `.dashboard` and `.project-card`, includes responsive rules, and visibly provides polished card styling with `border-radius` and `box-shadow`.

### Phase 2 — Dashboard markup, data loading, and rendering

**Owner:** Coder  
**Files:** `app/index.html`

1. Create the accessible static document shell with title, viewport metadata, a visible **Project Pulse** heading, a stylesheet reference, and semantic main content.
2. Reference/load `project-data.json` and render one visible `.project-card` for each item in `projects`.
3. Display every required field: project name, owner, status, recent activity, and priority.
4. Use the class hooks established in Phase 0 so the Designer’s CSS applies without edits to `styles.css`.
5. Provide clear user-facing loading and error states for unavailable, malformed, empty, or unexpected project data. Do not leave a blank dashboard or silently fail.
6. Keep rendering deterministic: preserve the source order unless an explicit ordering rule is documented, and avoid non-deterministic generated content.

**Dependencies:** Phase 1A must be complete because this phase consumes the JSON schema and source path. Phase 1B should be complete or its final hook contract must be confirmed before markup is finalized.  
**Completion condition:** A served dashboard visibly renders cards from the JSON source, uses `.dashboard` and `.project-card`, and degrades with readable error feedback when data cannot be loaded.

### Phase 3 — Orchestrator integration and verification

**Owner:** Orchestrator  
**Files modified:** none

1. Review all four implementation files against this plan and the Project Pulse brief.
2. Confirm agents stayed within their scopes and no integration issue was introduced.
3. Start the **Run Project Pulse Dashboard** launch configuration.
4. Verify that the browser opens `index.html` directly and the visible first view is the Project Pulse dashboard.
5. Perform the functional, responsive/accessibility, data-integrity, and launch checks below.
6. Report completed validation, failures, remaining risks, and any follow-up work without changing files directly.

**Dependencies:** Phases 1A, 1B, and 2 must all be complete.  
**Completion condition:** All measurable validation expectations pass, or any failure is reported with the responsible file and owner.

## Explicit inter-task dependencies

| Consumer task | Depends on | Why |
| --- | --- | --- |
| Phase 1A data creation | Phase 0 contract | The JSON field names must match the renderer and card semantics. |
| Phase 1B styling | Phase 0 contract | CSS needs stable `.dashboard` and `.project-card` hooks and field categories. |
| Phase 2 HTML/rendering | Phase 1A | The renderer needs the final JSON path, top-level `projects` key, and required record fields. |
| Phase 2 HTML/rendering | Phase 1B contract/completion | Markup needs the agreed visual hooks; final review should use the actual stylesheet. |
| Phase 3 integration | Phases 1A, 1B, and 2 | End-to-end verification requires data, presentation, behavior, and launch support together. |

## Parallel and sequential work

### May run in parallel

- **Phase 1A (Coder: data and launch configuration)** and **Phase 1B (Designer: stylesheet)** may run in parallel after Phase 0.
  - Their file scopes do not overlap.
  - Both use the shared contract but do not require the other’s completed file.
  - This shortens delivery time without risking conflicts.

### Must run sequentially

- **Phase 0 must precede all implementation work** because it establishes field names, class hooks, ownership, and launch expectations.
- **Phase 2 must follow Phase 1A** because `index.html` depends on the final JSON schema and data path.
- **Phase 2 should follow Phase 1B completion or a confirmed final CSS contract** because Coder must use the exact agreed hooks and avoid requiring Designer to edit HTML.
- **Phase 3 follows all implementation phases** because integration testing is meaningful only when the JSON, HTML, CSS, and launch configuration are present together.
- No two agents may edit the same file simultaneously. Any required correction returns to that file’s assigned owner, followed by another Orchestrator verification pass.

## Edge cases and risks to handle

1. **Direct-file access:** `fetch` of JSON may fail from `file://`; validate through the HTTP launch configuration, not by double-clicking `index.html`.
2. **Bad data:** Handle a missing JSON file, invalid JSON, a missing `projects` key, a non-array `projects` value, an empty array, and records with missing required fields. Present an understandable in-page state rather than failing silently.
3. **HTML safety:** Treat JSON values as content, not markup; render user-facing data safely so a data value cannot inject HTML.
4. **Variable content length:** Long project names, owner names, activity summaries, and priorities must wrap without overlap, clipping, or broken card alignment.
5. **Unknown statuses/priorities:** Continue to show the textual values even if they are not among anticipated badge variants; use a neutral fallback appearance.
6. **Color and contrast:** Status/priority must have visible text labels and sufficient contrast; badge color alone cannot carry meaning.
7. **Narrow viewports:** Ensure card content remains legible without horizontal page scrolling at a 320 px viewport.
8. **Server lifecycle:** The preview server should be stoppable after verification; avoid port or lingering-process ambiguity in the launch configuration.
9. **Strict configuration syntax:** JSON in `project-data.json` and `.vscode/launch.json` cannot contain comments, trailing commas, or other non-JSON syntax.

## Measurable validation expectations

### Functional validation

- The served page has the document title and visible primary heading **Project Pulse**.
- `app/index.html` references `styles.css` and `project-data.json`.
- The dashboard renders one `.project-card` per valid project object in `projects`.
- Each rendered card visibly includes `name`, `owner`, `status`, `recentActivity`, and `priority`.
- Cards, status badges, priority/risk treatment, and readable spacing are visible in the first dashboard view.
- Loading, empty-data, and data-load failure states are understandable and do not leave an unexplained blank page.

### Responsive and accessibility validation

- At 320 px, 768 px, and a desktop width of at least 1280 px, content remains readable; cards do not overlap and the page has no unintended horizontal scrolling.
- `.dashboard` and `.project-card` are present in `app/styles.css`.
- The stylesheet uses responsive layout rules plus visible `border-radius` and `box-shadow` card treatment.
- The HTML uses semantic document structure, one clear primary heading, and appropriate landmark/content grouping.
- Status and priority have text equivalents in addition to color; normal text and meaningful UI states meet accessible contrast expectations.
- Any introduced control can be reached by keyboard and has a visible focus indicator.

### Data integrity validation

- `app/project-data.json` parses as strict JSON.
- The top-level object has a `projects` array.
- The array contains multiple records appropriate for the dashboard demonstration.
- Every record has non-empty string values for `name`, `owner`, `status`, `recentActivity`, and `priority`.
- The visible card count matches the number of valid rendered project records, and the displayed field values match the JSON source.

### Launch and debug verification

- `.vscode/launch.json` parses as strict JSON with no comments.
- A configuration named exactly **Run Project Pulse Dashboard** exists.
- It runs `python3 -m http.server 5500` with `cwd` equal to `${workspaceFolder}/app`.
- `serverReadyAction` opens `http://localhost:%s/index.html`.
- Running the configuration opens the Project Pulse dashboard frontend rather than a directory listing.
- The server can be stopped cleanly after the verification run.

## Open questions

1. The brief requires priority **or risk** information while the data contract explicitly requires `priority`; this plan treats `priority` as the required field and presents it as priority/risk in the UI. Confirm whether a separate `risk` field is desired before extending the schema.
2. The brief does not prescribe exact project names, valid status vocabulary, priority levels, or project count. The Coder should use several representative records and allow unknown status/priority values to degrade gracefully.
3. The initial static scope does not include filtering, sorting, persistence, edit controls, or a backend. These should remain out of scope unless explicitly requested in a subsequent phase.
