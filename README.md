# IT PMO Board

A single-page Kanban board for a fictitious bank's internal IT PMO. It is a demo and training tool, written in vanilla HTML, CSS and JavaScript in one file, `index.html`, with no build step or dependencies.

## Features

- Four columns: Backlog, In Progress, Blocked, Done, each with a live task count.
- Task cards show ID, title, project, assignee, priority, due date and category, with an Overdue badge for late tasks.
- Drag and drop between columns, plus a keyboard-accessible "Move" dropdown on each card.
- Inline "Delete? Yes / No" confirmation (no browser dialogs).
- Add Task form with client-side validation and a toast on success.
- Filters by project, assignee and priority, and a live summary strip.
- Progress panel: a stacked overall completion bar plus a bar per project; click a project to filter the board and chart.
- Burndown chart (inline SVG) with an ideal line, remaining work, a Today marker and scope changes. Hover it or use the arrow keys to read a day; switch between story points and task count.
- Each task has an effort estimate in story points; click a card for its details.
- Optional email notification for new tasks through [FormSubmit](https://formsubmit.co).

Live demo: https://ofof.github.io/it-pmo-board/

## Run

Open `index.html` in a browser. No server is needed.

## Data

Board state lives in memory only. Refreshing the page resets it to the 13 seeded demo tasks (with completion dates, so the burndown has history).

## Email notifications

Set `FORMSUBMIT_ENDPOINT` at the top of the script in `index.html` to your own address. FormSubmit sends a one-time activation email on the first submission; notifications are delivered only after you click the link in it. If the request fails, the card is still added and a warning toast is shown.
