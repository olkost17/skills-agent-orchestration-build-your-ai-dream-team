# Project Pulse Dashboard Implementation Plan

## Summary

Build Mona's Project Pulse as a small, static, contributor-friendly dashboard. It
will present multiple project cards with the project name, owner, status, recent
activity, and priority/risk. The page will use accessible semantic HTML, polished
responsive CSS, and data loaded from a top-level `projects` array in
`app/project-data.json`. A VS Code launch configuration will serve `app/` with
Python's static server and open `index.html`, so the first browser view is the
dashboard rather than a directory listing.

The repository's exercise checks are primarily file, phrase, JSON, and
configuration checks. The implementation should satisfy those checks while also
being manually tested in a browser.

## Ordered implementation steps

1. **Confirm constraints and establish ownership.** Orchestrator reviews
   `.github/project-pulse-brief.md`, all custom agent instructions, the exercise
   steps, and `scripts/validate-exercise.sh`. Orchestrator records the scope
   below and keeps the Designer and Coder from editing the same file
   simultaneously.
2. **Produce the design direction.** Designer defines the information hierarchy,
   card anatomy, status/priority visual treatment, responsive breakpoints,
   keyboard and screen-reader behavior, color contrast, and loading/error
   presentation. Designer should give the Coder concrete class and markup
   guidance, including `.dashboard` and `.project-card`.
3. **Prepare representative data.** Coder creates
   `app/project-data.json` with several realistic projects. Every item has
   non-empty `name`, `owner`, `status`, `recentActivity`, and `priority`
   values, with statuses and priorities suitable for visible badges and
   accessible text.
4. **Implement the page.** Coder creates `app/index.html`, using the exact
   visible title “Project Pulse”, semantic landmarks and headings, a dashboard
   container, a project-card template/render target, and references to
   `styles.css` and `project-data.json`. Since this is a three-file static app,
   a small explicit inline script may fetch the JSON and render cards; no
   unassigned JavaScript file should be introduced.
5. **Implement the visual system.** Coder creates `app/styles.css` from the
   Designer's direction. Include `.dashboard`, `.project-card`, rounded cards,
   `box-shadow`, readable spacing, clear status and priority treatments,
   focus styles, and a responsive layout that remains usable on narrow screens.
6. **Make the app runnable.** Coder creates `.vscode/launch.json` as strict JSON
   with no comments. Add a configuration named **Run Project Pulse Dashboard**,
   set `cwd` to `${workspaceFolder}/app`, use `python3 -m http.server 5500`,
   and configure `serverReadyAction` to open
   `http://localhost:%s/index.html`.
7. **Integrate and review.** Orchestrator reviews all four assigned outputs
   against the brief and the Designer's decisions, checks that field names in
   the JSON, rendering logic, CSS hooks, and launch settings agree, and requests
   fixes from the Coder before validation if they do not.
8. **Validate and hand off.** Run the static checks, start the configured
   preview, inspect browser behavior at desktop and mobile widths, check
   accessibility basics, stop the server, and report results and remaining
   risks. Do not stage, commit, or push as part of this implementation plan.

## File assignments

| File | Owner | Required responsibilities and acceptance criteria |
| --- | --- | --- |
| `app/index.html` | Coder, informed by Designer | Semantic Project Pulse shell; exact title text; stylesheet and JSON references; visible project-card markup/render target; display of `name`, `owner`, `status`, `recentActivity`, and `priority`; loading, empty, and error messaging; accessible labels and focusable elements. |
| `app/styles.css` | Coder, following Designer's visual direction | `.dashboard` and `.project-card` selectors; polished card-based layout with `border-radius` and `box-shadow`; badges, hierarchy, spacing, contrast, focus states, and responsive rules. |
| `app/project-data.json` | Coder | Valid JSON with a top-level `projects` array; multiple representative records, each containing `name`, `owner`, `status`, `recentActivity`, and `priority`. Keep content contributor-friendly and safe to render as text. |
| `.vscode/launch.json` | Coder | Strict comment-free JSON; configuration name **Run Project Pulse Dashboard**; Python server command `python3 -m http.server 5500`; `cwd` `${workspaceFolder}/app`; `serverReadyAction` URL `http://localhost:%s/index.html`, targeting the frontend. |

The Designer owns design decisions and review, not implementation ownership of
these files. The Coder owns the implementation edits so that HTML/CSS/data and
launch behavior remain coordinated.

## Designer responsibilities

- Turn the brief into a clear contributor workflow: scan the title and summary,
  compare project cards, then identify owner, status, activity, and risk quickly.
- Define a consistent card structure and badge vocabulary without relying on
  color alone; pair status/priority colors with text and accessible names.
- Specify typography, spacing, contrast, card states, responsive grid behavior,
  and visible keyboard focus.
- Recommend semantic landmarks, heading order, `aria-live` behavior for loading
  or errors, and meaningful empty-state copy.
- Review the Coder's rendered result at narrow and wide viewport sizes and
  identify usability or accessibility regressions.

## Coder responsibilities

- Implement only the four assigned files and follow the Designer's decisions.
- Keep JSON valid and deterministic; render values as text rather than injecting
  untrusted HTML.
- Fetch `project-data.json` relative to the page, render one `.project-card` per
  project, and expose all required fields in the UI.
- Provide explicit loading, malformed-data, fetch-failure, and empty-project
  states that do not leave a blank page.
- Use responsive, accessible HTML/CSS with readable contrast, keyboard focus,
  reduced-motion consideration where applicable, and no dependency on a build
  tool or external service.
- Create the exact launch configuration and smoke-test it locally.

## Dependencies and ordering decisions

- The brief and agent instructions are prerequisites for both specialist
  activities.
- Designer's direction must precede the final HTML/CSS implementation, but it
  can be prepared in parallel with Coder's initial representative JSON because
  those scopes do not overlap.
- JSON schema and sample records must be settled before the Coder finalizes
  rendering logic. HTML and CSS can be drafted in parallel only if the
  Designer's class/markup contract is already agreed; the safer integration
  path is sequential HTML structure first, then CSS.
- `launch.json` can be authored in parallel with HTML/CSS after the required
  server command and port are agreed, because it has a separate file scope. Its
  browser smoke test must wait until `app/index.html` exists.
- Integration review and browser validation are sequential after all Coder
  outputs are present. Fixes and re-validation are also sequential.

## Edge cases and error states

- Missing, empty, malformed, or non-array `projects` data: show an informative
  empty/error state rather than throwing a blank-page exception.
- A record missing a required field: use a safe fallback such as “Not provided”
  and keep the other cards rendered.
- Network/fetch failure or opening the HTML through an unsupported `file://`
  context: show a clear “unable to load project data” message and instruct the
  user to use the launch configuration/static server.
- Zero projects: show an intentional empty state with preserved dashboard
  structure.
- Long project names, activity text, owner names, and unexpected characters:
  allow wrapping and render as text without layout overflow or HTML injection.
- Narrow screens, zoom, keyboard-only navigation, and reduced-motion settings:
  preserve readable order, focus visibility, and usable tap targets.
- Unknown status or priority values: retain the text and use a neutral visual
  treatment rather than relying on a missing CSS class.
- Port 5500 already in use: report the launch failure explicitly; do not silently
  claim the browser preview passed.

## Validation expectations

### Static files and JSON/configuration

- Confirm all four assigned files exist.
- Run `python3 -m json.tool app/project-data.json` and
  `python3 -m json.tool .vscode/launch.json`.
- Inspect that JSON has the exact top-level `projects` key and that every sample
  record has `name`, `owner`, `status`, `recentActivity`, and `priority`.
- Check `index.html` contains “Project Pulse”, references `styles.css` and
  `project-data.json`, uses `project-card`, and renders the status,
  `recentActivity`, and priority fields.
- Check `styles.css` contains `.dashboard`, `.project-card`,
  `border-radius`, and `box-shadow`, plus responsive and focus styling.
- Check `launch.json` is comment-free strict JSON, names **Run Project Pulse
  Dashboard**, uses the app working directory and `python3 -m http.server 5500`,
  and opens `http://localhost:%s/index.html`.

### Browser behavior

- Start **Run Project Pulse Dashboard** in VS Code and confirm the browser opens
  `/index.html`, not the directory root.
- Confirm multiple cards visibly render from `project-data.json` and each
  required field is readable.
- Exercise loading, successful, empty, malformed, and fetch-failure states when
  practical; check the console for unexpected errors.
- Test a desktop viewport, a narrow/mobile viewport, browser zoom, keyboard
  navigation, visible focus, and sufficient contrast. Confirm card content wraps
  without horizontal overflow.
- Stop the preview server after the smoke test.

### Repository validation

- Run `bash scripts/validate-exercise.sh` from the repository root.
- Treat a passing script as necessary but not sufficient: it validates the
  exercise repository and required static phrases/JSON, while the manual browser
  checks validate runtime behavior. Record any unrelated pre-existing failure
  separately rather than weakening the implementation.

## Open questions

- No product source was provided for real project records, so the Coder should
  use clearly labeled representative sample data unless Mona supplies canonical
  projects.
- The brief does not specify a framework, browser automation tool, branding, or
  exact status taxonomy; keep the app dependency-free and use a small,
  documented set of human-readable values.
- The brief does not require filtering, sorting, editing, persistence, or a
  backend. Defer those features unless a later requirement adds them.
