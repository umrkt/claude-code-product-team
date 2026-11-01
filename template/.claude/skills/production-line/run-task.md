# Run one task

Each worker ends with a hand-back block (`RESULT`, `BRANCH`, `WORKTREE`,
`COMMIT`, `SUMMARY`, `FINDINGS`; the Developer also `DECISIONS`). Always use
`BRANCH` and `WORKTREE` from the Developer's latest hand-back; the worktree
path changes on every Developer dispatch.

**Read the task first.** Workers never read Jira; they see only the brief.
Before every dispatch, read the task (`jira.md`, "Read one"; it shows the
description and all comments). Carry into the brief every comment that
changes scope or criteria: user decisions, PO scope notes, acceptance
criteria changes. Developer fix briefs also get the latest Review/QA
findings. Reviewer and QA briefs do not get Developer claims or earlier
review verdicts (blind hand-off below).

**Blind hand-off.** Reviewer and QA briefs contain only: Jira key, acceptance
criteria, branch, worktree, commit, and shared team knowledge (user
decisions, known environment limits, accepted PO decisions). Never pass the
Developer's SUMMARY or claims ("tests pass", "I fixed X") to Reviewer or QA;
they find out for themselves. You may name areas to look at, as questions,
not as claims.

**Decisions.** Post the Developer's `DECISIONS` line (if not "none") as a
Jira comment. Product or architecture decisions you accept also go into
`docs/product/DECISIONS.md` (one dated line each, task key, why).

**Sending work back to the Developer** (fix or continue brief): first run
`git worktree remove <latest worktree>` from the main checkout, so the branch
is free to check out in the new worktree. If git refuses because of
uncommitted changes, stop and report to the user; never use `--force`. If it
says the worktree is locked, `git worktree unlock <path>` and retry once.

**Fix rounds.** Count send-backs per task (Review and QA together). Note the
round in the Jira comment ("fix round 2/5"). If Review or QA still fails after
the fifth fix round, do not send it back again: comment what kept failing,
leave the status, and report to the user.

**Plan first for risky tasks.** A task is risky if it changes what the
product accepts as truth (core rules, data contracts, expected answers'
rules, public outputs), or touches more than three packages. For those, the
first dispatch is a plan brief: the Developer hands back `RESULT: PLAN` with
no code. Move to `Review` and dispatch `reviewer` with a plan-review brief.
`CHANGES_REQUESTED` → back to `In Development` with a plan-fix brief (plan
rounds do not count as fix rounds). `PASS` → back to `In Development` and
dispatch the Developer with a continue brief that includes the approved
plan. Mark the task risky in the Jira comment when you start it. A plan is
work on the task: it counts for the file-overlap rule.

1. Move the task from `To Do` to `In Development`. Dispatch `developer` with
   a new-task brief (or a plan brief if risky): Jira key, summary,
   acceptance criteria.
2. Developer `BLOCKED` → leave the status, add a Jira comment with the
   blocker, and stop for this task (report it). Developer `DONE` → move to
   `Review` and dispatch `reviewer` with key, criteria, branch, worktree.
3. Reviewer `CHANGES_REQUESTED` → move back to `In Development` and send the
   findings to the `developer` as a fix brief; then `Review` again.
   Reviewer `PASS` → move to `QA` and dispatch `qa` with the same inputs.
4. QA `FAIL` → move back to `In Development`, fix brief with QA findings, then
   `Review` and `QA` again. QA `BLOCKED` → comment what is missing; if you can
   provide it (e.g. a download or a fixture file QA cannot write itself), do
   and re-run QA; otherwise stop and report.
   QA `PASS` → move to `Waiting For Release` and continue with `release.md`.

Count distinct approaches per problem across fix loops. After five without
success, stop and report what was tried.

Every brief repeats the command rules from `AGENTS.md` (one plain command
per Bash call, no `cd`, pipes or env prefixes; `python3 -I -c` allowed), so
workers do not stall on permission prompts.
