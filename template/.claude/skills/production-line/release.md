# Release one task

One task at a time, from the main checkout (not a worktree), which stays on
`main`.

0. `git status --porcelain` must be empty; otherwise stop and report.
1. `git fetch origin`, then `git pull --ff-only`.
2. `git merge --no-ff feature/<KEY>-<slug> -m "<KEY>: <summary>"`
   - Conflict → `git merge --abort`, move the task to `In Development`, send
     the `developer` a fix brief to rebase on `main` (`run-task.md`, "Sending
     work back"), then `Review` and `QA` again.
3. `<TEST_CMD>` on `main` (plus any suite the change touched).
   - Red → `git reset --hard ORIG_HEAD` (only this local, unpushed merge),
     then the same send-back path as a conflict. Never push red.
4. Green → `git push origin main`. Never force-push.
5. Jira comment: what changed, merge SHA, checks run and their result. Move
   the task to `Done`. If the task settled a product or architecture
   decision, add it to `docs/product/DECISIONS.md` on `main`.
6. Clean up: `git worktree remove <worktree>` (if locked,
   `git worktree unlock <worktree>` first; never `--force`) and
   `git branch -d feature/<KEY>-<slug>`. Also delete this task's leftover
   automatic branches: `git branch --list 'worktree-agent-*'`, then
   `git branch -d <name>` for each one whose agent worked on this task (the
   names match the worktree paths in the hand-backs). `-d` refuses unmerged
   branches; leave those and never use `-D`.
7. If another task was parked because its files overlap this one, it may
   start now: it rebases on the new `main` before Review and QA.
