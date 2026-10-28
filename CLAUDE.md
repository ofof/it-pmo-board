# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A single-file IT PMO Kanban demo/training app for a fictitious bank: `index.html` (markup, `<style>` and `<script>` all inline). There is no build, lint or test tooling. Use a neutral "IT PMO" text wordmark only, never real bank logos or branding.

## Running

Open `index.html` directly in a browser (`xdg-open index.html`); no server is needed. The in-app browser pane refuses `file://` URLs, so to drive it there, serve the folder (`python3 -m http.server 8123 --bind 127.0.0.1`) and navigate to `http://localhost:8123/index.html`.

## Hard constraints (from the original spec)

- Vanilla HTML/CSS/JS only: no frameworks, bundler or npm, and no external resources (no CDN, web fonts or image files). Icons are Unicode glyphs or inline SVG.
- No persistence of any kind: no `localStorage`, `sessionStorage`, IndexedDB or cookies. A refresh must reset the board to the seeded data, and the UI says so.
- No `!important` in CSS; no `alert()`/`confirm()` (inline errors, toasts and an inline "Delete? Yes / No" toggle instead).
- The only network call is the FormSubmit AJAX endpoint in `notifyNewTask()`. `FORMSUBMIT_ENDPOINT` at the top of the script holds the notification address (placeholder `YOUR_EMAIL@example.com`); never send it anywhere else. FormSubmit needs a one-time activation email click before deliveries start.

## Architecture

State-driven rendering: `state = { tasks, filters }` is the source of truth, with transient view state in a separate `ui` object (`confirmDeleteId`, `draggingId`). All card DOM is produced by `renderBoard()` / `renderCard()`, which rebuild the whole board and the header summary. Actions (`addTask`, `moveTask`, `deleteTask`) mutate state and then call `renderBoard()`; do not edit card DOM directly.

- Tasks carry `startDate`, `dueDate`, `progress`, `points`, `createdAt` and `completedAt`. `renderBoard()` is the single render entry point: it shows either `renderWaterfall()` (Gantt-style rows positioned as percentages of the visible window, `ui.span` 28 or 56 days) or `renderKanban()`, then `renderSummary()` and `renderInsights()` (sidebar progress bars plus the SVG burndown, which follows the active filters and the Points/Tasks toggle). Chart hover only re-renders the chart (`renderChart()`).
- Every user-supplied string must go through `escapeHtml()` before it enters an HTML template string.
- Interactions use event delegation: clicks, Move menu and keyboard on `#views`, Kanban drag and drop on `#board`, and pointer-drag rescheduling of waterfall bars on `#waterfall`. Don't attach per-element listeners, since the views are recreated on every render. The details dialog edits state live via `moveTask`, `setProgress` and `setDates`.
- Use selectors like `.board[hidden]` (not `!important`) when a class sets `display` on something toggled with `hidden`.
- Task IDs come from `generateTaskId()` (`ITPM-####`, counter `nextIdNumber`); seed dates are offsets from today so overdue badges and the burndown always demo correctly.
- Submit is optimistic: the task is added first, then `notifyNewTask()` runs; a failure only shows a warning toast.
