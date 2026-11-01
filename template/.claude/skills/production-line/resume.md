# Resume ("continue production")

Do this at the start of every production run, before picking new work.

0. Preconditions, in the main checkout: `git status --porcelain` is empty,
   the current branch is `main`, and after `git fetch origin` local `main`
   equals `origin/main`. If any fails, stop and report: new worktrees branch
   from `origin/main`, so an unpushed `main` gives workers stale instructions.
1. List the board (`jira.md`), `git worktree list` and
   `git branch --list 'feature/*'`.
2. A task counts as released when `git log main --oneline --grep '^<KEY>:'`
   finds its release merge commit (`release.md` writes `<KEY>: <summary>`).
3. Ignore `Done` and `To Do` tasks here. Reconcile the others so Jira matches
   git, and note each change for the report:
   - released but not `Done` → move to `Done`;
   - `Waiting For Release`, not released, branch exists → run `release.md`;
   - `In Development`/`Review`/`QA`/`Waiting For Release`, not released, and
     no `feature/<KEY>-*` branch → move back to `To Do`.
4. Continue each remaining in-progress task from its status on its existing
   branch; never open a second branch or restart it:
   - `In Development` → `git worktree remove <its worktree>` if it has one
     (`run-task.md`, "Sending work back") and dispatch `developer` with a
     continue brief naming the branch;
   - `Review` / `QA` → dispatch `reviewer` / `qa` with the branch and its
     worktree from `git worktree list` (if none, send a continue brief first);
   - `Waiting For Release` → `release.md`.
5. Run a rolling queue: keep up to 3 tasks in progress at once. Whenever a
   slot frees (a task reaches `Done`, or stops blocked for the user), pick
   the next `To Do` task in board order that is not blocked by an open task
   (Jira "is blocked by" links) and whose likely files do not overlap any
   task in progress, plans included. If unsure about overlap, wait for the
   slot that frees it. Before picking, check whether a retro is due.
6. Run each task with `run-task.md`; Developers run in the background,
   concurrently. Report to the user when nothing more can be started and
   the running work is finished or blocked, or on a stop rule in
   `SKILL.md`. No per-task chat notes; the Jira board shows progress.
