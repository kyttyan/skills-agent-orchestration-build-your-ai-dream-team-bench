# Project Pulse Dashboard Implementation Plan

## Summary

Build Mona’s lightweight, static Project Pulse dashboard so contributors can quickly see active projects, owners, status, recent activity, priority or risk, and a short summary. Use plain HTML, CSS, and JSON; no framework or additional dependency is needed. Serve the app from `app/` and open `index.html` using the **Run Project Pulse Dashboard** VS Code launch configuration.

The project brief and Step 3 instructions define required content and launch behavior. The existing Step 2 and Step 3 workflows check for expected paths, key phrases, and valid JSON, but do not verify that the dashboard renders correctly in a browser.

## Implementation phases and file ownership

### Phase 1 — Agree on design and data contract

**Owner: Designer**, with Orchestrator coordinating. **No repository files are edited in this phase.**

- Define the dashboard’s information hierarchy, card layout, responsive behavior, typography, spacing, and accessible status/priority treatment.
- Specify CSS hooks and the content each card needs to display.
- Recommend a small, consistent set of status and priority labels.
- Treat `summary` as a recommended additional project field: the brief asks for contributor-friendly summaries but does not list `summary` among the required JSON fields.
- Hand off the design decisions to Coder before HTML and CSS implementation begins.

### Phase 2 — Create the app and launch configuration

**Owner: Coder.** Coder has exclusive edit ownership of all four implementation files; Designer provides guidance but does not edit them. This avoids overlapping edits.

| File | Assignment |
|---|---|
| `app/project-data.json` | Create a top-level `projects` array with sample entries. Every entry must include `name`, `owner`, `status`, `recentActivity`, and `priority`. Include `summary` to meet the contributor-friendly summary requirement. Keep labels and values consistent. |
| `app/index.html` | Create the semantic dashboard structure, exact visible title **Project Pulse**, and references to `styles.css` and `project-data.json`. Load the JSON and render a visible `.project-card` for each project, showing owner, status, activity, priority, and summary. Include clear loading, empty-data, and fetch/parse error states. |
| `app/styles.css` | Implement the agreed visual design and responsive layout. Include `.dashboard` and `.project-card` hooks, readable spacing, visible status/priority treatments, rounded corners, and shadows. Ensure color is not the only way to distinguish status or priority. |
| `.vscode/launch.json` | Create strict JSON with no comments and a **Run Project Pulse Dashboard** configuration. Serve from `${workspaceFolder}/app`, use the exercise-specified `python3 -m http.server 5500` command, and configure `serverReadyAction` to open `http://localhost:%s/index.html`. Use a launch type compatible with the available VS Code setup. |

### Phase 3 — Integrate and validate

**Owner: Coder**, with Orchestrator reviewing the result. No additional file ownership is needed.

- Confirm the markup, CSS hooks, JSON field names, and rendered card content agree.
- Run the app using **Run Project Pulse Dashboard** and verify it opens the dashboard page rather than a directory listing.
- Resolve integration issues before handoff.

## Dependencies and ordering

1. Designer’s design direction and Orchestrator’s data-contract confirmation precede HTML and CSS implementation.
2. The JSON shape and labels must be agreed before connecting the HTML renderer to the data.
3. The HTML structure and CSS hooks must align; validate them together.
4. The launch configuration depends on the agreed `app/` working directory and the intended `index.html` URL.
5. Final browser validation depends on all four implementation files being present.

## Parallel versus sequential work

- **Sequential:** Designer completes the design handoff before Coder implements the HTML/CSS. Agree on the data contract before connecting the renderer.
- **Parallel:** After the data and launch requirements are confirmed, Coder may prepare `app/project-data.json` and `.vscode/launch.json` independently because they have separate file scopes. Designer may review visual decisions in parallel but must not edit Coder-owned files.
- **Sequential integration:** Coder connects the HTML renderer to the data, aligns CSS with the actual markup, and then performs end-to-end validation. Do not parallelize edits to `index.html` and `styles.css` across agents.

## Edge cases and risks

- Fetching JSON from `file://` may fail in browsers; use the HTTP server launch configuration for preview.
- Handle failed fetches, invalid JSON, an empty `projects` array, and missing or blank optional content without leaving a blank or broken dashboard.
- Render data as text rather than injecting untrusted values as HTML.
- Keep status and priority labels consistent and understandable; convey them with text as well as color.
- Check narrow viewports, long project names, long activity text, and keyboard-visible focus.
- Port `5500` may already be in use. If launch fails, identify the conflict and keep the configured port, launch URL, and readiness detection consistent if a port change is approved.
- VS Code launch properties depend on the selected launch type. Confirm the required launch support is available; do not assume an uninstalled Python debugger extension.
- Python’s built-in `http.server` is suitable for this local static preview, not a production hosting solution.
- Codespaces browser/port-forwarding behavior may differ from local VS Code; verify the launched URL in the target environment.

## Validation expectations

- Confirm all four assigned files exist.
- Parse `app/project-data.json` and `.vscode/launch.json` as JSON; the Step 3 workflow uses `python3 -m json.tool` for both.
- Check for the expected title, stylesheet/data references, `.dashboard` and `.project-card` hooks, required data fields, and launch name/target. The Step 2 and Step 3 workflows provide repository-specific phrase checks.
- Run **Run Project Pulse Dashboard** in VS Code. Confirm the browser shows Project Pulse at `/index.html`, the project cards render from JSON, and no directory listing appears.
- Inspect loading, empty, and error states; test at narrow and wide viewport sizes and with keyboard navigation.
- Treat workflow phrase checks as necessary but insufficient: they do not prove the fetch succeeds or that the UI works in a browser.

## Open questions

- The brief does not supply real project names or activity. Use clearly illustrative sample data unless Mona provides actual project details.
- The brief requires a short summary but does not list `summary` as a required JSON field. This plan recommends adding it; confirm if the schema must remain limited to the five explicitly listed fields.
- Status and priority vocabularies are unspecified. Choose a small consistent set during the design/data-contract phase.
- Confirm the launch type and browser-opening behavior in the target Codespace/VS Code environment.
