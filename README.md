# IT PMO Board

A single-page Kanban board for a fictitious bank's internal IT PMO. It is a demo and training tool, written in vanilla HTML, CSS and JavaScript in one file, `index.html`, with no build step or dependencies.

## Features

- Waterfall (Gantt-style) timeline: tasks grouped by project and cascading by start date, with a Today line, status-coloured bars and a progress fill. Switch between a 4-week and 8-week range, and drag a bar sideways to reschedule it.
- Kanban board view (Backlog, In Progress, Blocked, Done) with native drag and drop and a keyboard-accessible Move menu.
- Task details dialog: change status, progress and dates; effort is tracked in story points.
- Add Task form with client-side validation and a toast on success.
- Sidebar with live summary counts, filters (project, assignee, priority) and progress bars; click a project bar to filter.
- Burndown chart (inline SVG) with an ideal line, remaining work and a Today marker. Hover it or use the arrow keys to read a day; switch between points and task count.
- Optional email notification for new tasks through [FormSubmit](https://formsubmit.co).

## Run

Open `index.html` in a browser. No server is needed.

## Data

Board state lives in memory only. Refreshing the page resets it to the 13 seeded demo tasks.

## Email notifications

Set `FORMSUBMIT_ENDPOINT` at the top of the script in `index.html` to your own address. FormSubmit sends a one-time activation email on the first submission; notifications are delivered only after you click the link in it. If the request fails, the card is still added and a warning toast is shown.
