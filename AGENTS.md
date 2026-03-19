# AGENTS.md

## Purpose
- This file is the working guide for coding agents operating in this repository.
- Keep changes minimal, readable, and consistent with the current no-build static app setup.
- Prefer improving existing patterns over introducing new frameworks or tooling.

## Repository Snapshot
- App type: single-page static web app.
- Main runtime file: `index.html` (contains HTML, CSS, and Vue logic).
- Static assets: `assets/` (`dark.svg`, `light.svg`, `favicon.svg`).
- Dependency loading: Vue is loaded from CDN (`vue.global.prod.js`) in `index.html`.
- Package/tooling manifests: none found (`package.json`, `pyproject.toml`, `go.mod`, etc. are absent).
- Existing AGENTS/assistant docs are minimal placeholders; this file is the primary actionable guide.

## Build, Lint, and Test Commands

### Current project reality
- There is no configured build step.
- There is no configured linter or formatter command.
- There is no configured automated test runner.

### Run locally (recommended)
```bash
python3 -m http.server 4173
```
- Then open `http://localhost:4173`.
- This is the safest default workflow for development and manual validation.

### Build command
- None.
- Do not invent a build pipeline unless explicitly requested.

### Lint command
- None.
- If style cleanup is needed, follow the conventions in this file manually.

### Test command
- None.
- Automated tests are not currently part of this repository.

### Run a single test (important)
- Not available right now because no test framework or test files are configured.
- If a test framework is introduced later, document exact single-test commands here immediately.

## Manual Verification Checklist
- Start local server with `python3 -m http.server 4173`.
- Verify theme toggle works and persists after refresh.
- Verify event name editing (click name, edit, blur/Enter to confirm).
- Verify date picker opens and selected date displays correctly.
- Verify timer input validation for hours/minutes/seconds bounds.
- Verify Start button disabled when total time is zero.
- Verify timer start, pause, resume, and reset states.
- Verify completion modal appears when countdown reaches zero.
- Verify keyboard behavior: `Escape` closes modal, focus remains usable.
- Verify layout on mobile widths (<= 520px and <= 360px).

## Code Style Guidelines

### General architecture
- Keep the project lightweight: prefer a single static page unless a split is clearly justified.
- Keep state logic inside Vue Composition API `setup()` unless refactoring is requested.
- Maintain clear sectioning in HTML/CSS/JS (template blocks, style sections, logic sections).

### Imports and dependencies
- Current codebase uses no ES module imports; Vue is consumed via global `Vue` from CDN.
- For CDN scripts, pin exact versions and include `integrity` + `crossorigin` when possible.
- Avoid adding new third-party dependencies for small tasks that can be solved with native APIs.
- If modularization is requested, prefer native ESM with explicit relative paths.

### JavaScript and Vue conventions
- Use `const` by default; use `let` only when reassignment is required.
- Use `ref`, `computed`, `watch`, `nextTick`, `onMounted`, `onUnmounted` patterns consistently.
- Keep reactive values in `camelCase` and return all template-used bindings from `setup()`.
- Prefer small, focused functions (`startTimer`, `pauseTimer`, `clampMinutes`, etc.).
- Use guard clauses for invalid state/inputs to keep logic linear.
- Keep timer state values explicit (`idle`, `running`, `paused`) and avoid hidden transitions.
- Preserve semicolons and double-quoted strings to match existing code style.
- Keep trailing commas in multiline arrays/objects/function calls.

### HTML conventions
- Use semantic structure (`main`, `header`, `section`, `footer`) as currently implemented.
- Keep indentation at 2 spaces.
- Keep attributes readable; split long tags across lines similar to existing markup.
- Preserve accessibility attributes (`aria-*`, `role`, `aria-live`, `aria-atomic`) when editing.
- Keep interactive elements keyboard accessible (`button` over clickable `div`).

### CSS conventions
- Keep styles in the existing `<style>` block unless a file split is explicitly requested.
- Use CSS custom properties for theming (`--bg-primary`, `--text-secondary`, etc.).
- Preserve theme scoping under `[data-theme="dark"]` and `[data-theme="light"]`.
- Keep class names in kebab-case.
- Keep transitions subtle and purposeful; do not add gratuitous animation.
- Preserve current responsive breakpoints and extend them carefully.

### Naming conventions
- Variables/functions: `camelCase`.
- Booleans: prefix with `is`, `has`, or explicit state adjectives (`hoursInvalid`).
- Computed display values: descriptive prefixes like `display*`, `formatted*`, `total*`.
- Event handlers: verb-led names (`onHoursInput`, `openDatePicker`, `confirmName`).
- CSS classes: `kebab-case`.

### Types and data modeling
- Codebase is plain JavaScript (no TypeScript).
- For non-trivial new data shapes, add lightweight JSDoc typedefs if helpful.
- Normalize user input at boundaries (parse, validate, clamp).
- Avoid implicit coercion for user-provided values.

### Error handling and resilience
- Use graceful fallbacks for browser APIs (existing `showPicker()` + `click()` fallback is a model).
- Use `try/catch` only where runtime failure is expected and recoverable.
- Do not silently ignore failures that affect UX; fail soft with a safe fallback.
- Clean up side effects (`clearInterval`, event listeners) in lifecycle teardown.

### Accessibility and UX quality bar
- Preserve live-region announcements for timer state changes.
- Ensure keyboard-only operation remains functional after any UI change.
- Keep focus management for modal interactions.
- Maintain minimum touch target usability for controls.
- Keep text contrast aligned with current dark/light themes.

### Content and assets
- Keep assets under `assets/` and use descriptive filenames.
- Prefer SVG assets for logos/icons where possible.
- Do not embed large binary blobs in source files.

## Rules from Cursor/Copilot Config
- `.cursor/rules/`: directory exists but is currently empty.
- `.cursorrules`: not present.
- `.github/copilot-instructions.md`: not present.
- Therefore, there are no additional Cursor/Copilot rule files to merge at this time.

## Agent Workflow Expectations
- Before major edits, read `index.html` sections you will touch to preserve conventions.
- Make focused changes; avoid broad reformatting unrelated to the task.
- Do not add infrastructure (bundlers, linters, test runners) unless explicitly requested.
- When introducing new commands/tooling, update this file in the same change.
- Prefer manual verification steps above when no automated tests exist.

## Quick Pre-PR Checklist for Agents
- Confirm app still runs via local static server.
- Confirm no regressions in timer core flows.
- Confirm accessibility attributes/keyboard flows remain intact.
- Confirm dark and light themes both still work.
- Confirm documentation updates match actual repository capabilities.
