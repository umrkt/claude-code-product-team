---
name: production-line
description: Use when the PO runs production on Jira tasks (resuming work, dispatching Developer/Reviewer/QA, moving statuses, merging and pushing) or gives the user a status report.
---

# Production line

The engineering-manager procedure the PO follows. Jira and git are the only
state; never keep a separate record.

## Flow

| Status | Who works | Leaves when |
|---|---|---|
| To Do | — | PO picks it → In Development |
| In Development | developer | DONE → Review; BLOCKED → stop and report |
| Review | reviewer | PASS → QA; CHANGES_REQUESTED → In Development |
| QA | qa | PASS → Waiting For Release; FAIL → In Development |
| Waiting For Release | PO | merged, tested and pushed → Done |
| Done | — | — |

Jira must always show the real status; the user tracks work from the board.

## Stop rules

Stop and report to the user (in their language) only when:

- the queue is done: nothing more can be started and running work is
  finished or blocked,
- a problem resisted five distinct approaches,
- a task failed Review or QA after its fifth fix round,
- a usage or rate limit was hit (see below),
- a retro is due (see below), or
- a question blocks all remaining work.

Collect non-blocking questions and suggestions for the report. Write each
one as a Jira comment starting `Question for the user:` on the task it
concerns, so it survives the session.

## Retro

Every 15 tasks that reach `Done` since the last retro, start no new task.
Let running tasks finish, then prepare and hold the retro with the user as in
`docs/product/retros/README.md`. Check the count before picking each new
task.

## Worker duration

Each completion notice shows the worker's duration. A worker that took over
45 minutes may have sat on a permission prompt the user did not see. Note
it in the next report with the worker and task, and look for the command
that needed approval (ask the user if unclear), so the rule or the brief
can be fixed.

## Usage or rate limit

Dispatch nothing new. Let running workers finish and handle their
hand-backs. Post a `State:` comment on every task that is not `To Do` or
`Done` (where it stands, branch, next step). Then tell the user.

## Steps — read the file when you reach it

1. `jira.md` — exact Jira commands and error handling. Read first.
2. `resume.md` — start of every "continue": reconcile, then pick a batch.
3. `run-task.md` — Developer → Review → QA loop with status moves;
   `qa-checks.md` — what QA must check.
4. `release.md` — merge, test, push, Done, cleanup.
5. `status-report.md` — the report format.
