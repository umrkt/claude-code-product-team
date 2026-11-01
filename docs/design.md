# Design

## Intent

A user who acts as CEO/CTO wants a product built by a small team of Claude
Code agents on their own Claude Code account. They talk only to a Product
Owner, ask for status reports or tell it to continue, and follow progress on
a Jira board.

An earlier attempt wrapped Claude Code in a large custom control layer:
authority rules, a SQLite state store, evidence hashing, acceptance gates, a
fake Jira and a blocking pre-tool hook. It never delivered a single real
task. This design replaces it with native Claude Code features only.

## Guiding ideas

- **One main loop does most of the work.** Procedures live in a skill that
  is loaded step by step. Subagents are used only where they earn it:
  parallel independent work, and verification by an agent that did not do
  the work. Split by context, not by org chart.
- **The finder is never the verifier.** Developer, Reviewer and QA are
  separate agents, and Reviewer and QA do not see the Developer's own
  account of the work.
- **Short entry point, step files on demand.** `SKILL.md` holds the flow and
  stop rules; each step (`resume`, `run-task`, `release`, ...) is a file the
  PO reads when it gets there.
- **Jira and git are the only state.** No ledger, no database. A new session
  reconciles the board with git and continues.

## Roles

| Role | How it runs | Responsibility |
|---|---|---|
| PO | main session (`agent` in `.claude/settings.json`) | talks to the user, researches, keeps the goal document, creates and orders tasks, runs the production line, merges and pushes, reports |
| Developer | subagent, own git worktree | implements one task on a feature branch, test first, fixes findings |
| Reviewer | subagent, read-only | fresh review of the diff (or plan) against the acceptance criteria |
| QA | subagent | fresh verification: build, tests, runs on real samples, negative paths |

The engineering manager is not a separate agent; it is a procedure inside
the PO. One fewer hand-off, and the three workers stay directly visible in
Claude Code's agent view.

## Authorization

The user approves the setup once. `CLAUDE.md` records that this approval
covers committing on feature branches, merging into `main`, pushing `main`
and moving Jira statuses. Force-push, history rewrites, deleting unmerged
branches, `sudo` and global configuration changes still need the user.
`.claude/settings.json` backs this with a deny list.

## Non-goals

- No custom control code, state database, evidence hashing or enforcement
  hooks.
- No new roles beyond PO, Developer, Reviewer, QA.
- Anti-goal: infrastructure progress reported as product progress. If the
  first real task does not reach `Done`, the setup failed.
