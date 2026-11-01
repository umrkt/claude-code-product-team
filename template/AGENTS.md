# Working agreement

Shared instructions for every agent working in this repository.

## Product goal

<PRODUCT_GOAL>

The full goal document is `docs/product/GOAL.md` (written by the PO with the
user on the first run).

## Team

- **PO**: the main session. Talks to the user, researches, owns the Jira
  backlog and runs the `production-line` skill (the engineering-manager
  procedure).
- **Developer**, **Reviewer**, **QA**: subagents dispatched by the PO.
  The agent that did the work never verifies it.

Jira (`<JIRA_KEY>` project) and git are the only state.

## Rules

- Talk to the user in <USER_LANGUAGE>, including status and task reports.
  Write Jira tasks and comments, specs, briefs and commit messages in English.
- Commit messages: one line, no AI attribution, no trailers.
- At most five distinct approaches per problem; then stop and report what was
  tried and why it failed.
- At most five fix rounds per task: if Review or QA still fails after the
  fifth send-back to the Developer, the task stops and goes to the user.
- Never fabricate expected results; derive them from the source.
- Tasks whose likely files overlap never run at the same time, not even a
  plan beside an implementation: the next one starts only after the earlier
  one is released to `main`. The PO does not bend this for speed.
- Retro every 15 tasks that reach `Done`. When one is due, no new task
  starts. Procedure, questions and results: `docs/product/retros/`.
- Bash commands must match the allowlist in `.claude/settings.json`, or the
  user gets a permission prompt. One plain command per Bash call, starting
  with the program name: no `VAR=value` prefixes, no `;`, `&&`, `|`, `$?`,
  `$VAR`, loops or heredocs. Use `-C`/`--prefix` instead of `cd`, read
  output in the next call instead of piping, and write files with
  Edit/Write. If a check truly needs an environment variable, prefer a tool
  flag; otherwise skip that check and report it.
- `python3 -I -c '<code>'` is allowed. Agents that can write files may
  prefer a script in the scratchpad run with `python3 -I <file>`; agents
  without a write tool (Reviewer, QA) use `-c` to create fixtures, edit
  scratch copies and read results.

### Free without asking

- Branches, commits, tests, builds and runs of the product on samples.
- Jira status moves and comments in the approved flow.
- Merging into `main` and pushing `main` to `origin` (see `CLAUDE.md`).
- Downloading build-time inputs (releases, package registries, public
  repositories) into git-ignored directories.

### Needs the user

- `sudo`, system-wide installs, global Claude or Git configuration changes.
- Force-push, history rewrites, deleting unmerged branches.
