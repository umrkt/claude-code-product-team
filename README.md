# Claude Code product team

A small software team made of Claude Code agents, built only from native
Claude Code features: one main session, three subagents, one skill and a
few Markdown rule files. No custom control code, no state database, no hooks.

You act as CEO/CTO. You talk only to the **Product Owner (PO)**. You say
"continue production" or "give me a status report", and you follow the work
on a Jira board.

| Role | Runs as | Does |
|---|---|---|
| PO | the main session | talks to you, owns the goal document and the Jira backlog, runs the production line (it is also the engineering manager), merges and pushes |
| Developer | subagent in its own git worktree | implements one task on a feature branch, test first |
| Reviewer | read-only subagent | fresh review of the diff against the acceptance criteria |
| QA | subagent | fresh verification: build, tests, and running the product on real samples |

Every task moves through six Jira statuses:

**To Do → In Development → Review → QA → Waiting For Release → Done**

Jira and git are the only state. Nothing is kept anywhere else, so any new
session can pick up where the last one stopped.

This setup grew out of a real project, where it took dozens of tasks from
backlog to `main`. The rules in it come from what went wrong along the way;
see [docs/lessons.md](docs/lessons.md).

## What is in here

```text
template/                      copy this into your repository
  AGENTS.md                    shared working agreement for every agent
  CLAUDE.md                    loads AGENTS.md, grants the standing authorization
  .claude/settings.json        main agent = po, permission allow/deny lists
  .claude/agents/              po, developer, reviewer, qa
  .claude/skills/production-line/
    SKILL.md                   flow, stop rules, retro, worker duration
    jira.md                    exact Jira commands (twg CLI)
    resume.md                  "continue": reconcile Jira with git, pick work
    run-task.md                Developer → Review → QA loop
    qa-checks.md               what QA checks
    release.md                 merge, test, push, Done, cleanup
    status-report.md           report format
  docs/product/
    DECISIONS.md               one dated line per product decision
    retros/README.md           retro procedure and questions
docs/
  design.md                    why it is built this way
  lessons.md                   rules learned in production
```

## Requirements

- [Claude Code](https://docs.claude.com/en/docs/claude-code) with subagents
  and git worktrees.
- A Jira project with the six statuses above, where any status can move to any
  status (a simple team-managed board works).
- A command-line Jira client. The template uses Atlassian's `twg` CLI. Any
  CLI works if you rewrite the commands in `jira.md`.
- Recommended: the [superpowers](https://github.com/obra/superpowers) plugin.
  The Developer follows its `test-driven-development` skill, and QA uses
  `verification-before-completion`.

## Quick start

1. Copy everything under `template/` into the root of your repository.
2. Replace the placeholders. Search for `<` in the copied files:

   | Placeholder | Example |
   |---|---|
   | `<PROJECT_NAME>` | `Acme API` |
   | `<PRODUCT_GOAL>` | two or three paragraphs: what the product does and how success is measured |
   | `<USER_LANGUAGE>` | `English`, `Turkish`, ... (chat language with you; written artifacts stay English) |
   | `<JIRA_SITE>` | `acme.atlassian.net` |
   | `<JIRA_KEY>` | `ACME` |
   | `<JIRA_BOARD>` | `3` |
   | `<TWG_PATH>` | `/home/you/.local/bin/twg` (absolute, so it matches the allowlist) |
   | `<BUILD_CMD>`, `<TEST_CMD>`, `<FORMAT_CHECK>` | `go build ./...`, `go test ./...`, `gofmt -l` |
   | `<RUN_PRODUCT>` | how QA runs your product on a sample |

3. Adjust `.claude/settings.json`. The allow list should cover exactly the
   programs your team runs (build tool, package manager, git, the Jira CLI),
   or agents will stop on permission prompts.
4. Log the Jira CLI in once yourself.
5. Start Claude Code in the repository. The main session is the PO. Tell it
   your goal; on its first run it writes `docs/product/GOAL.md` with you,
   creates the backlog and takes the first real task through all six statuses.
6. From then on: "continue production", "give me a status report".

## Core rules (short)

- The agent that did the work never verifies it. Reviewer and QA get a
  **blind hand-off**: acceptance criteria, branch and worktree, but never the
  Developer's own claims.
- **Plan first** for risky tasks: the Developer hands back a plan with no
  code, the Reviewer reviews the plan, then the Developer implements.
- At most **five fix rounds** per task and **five distinct approaches** per
  problem; then the PO stops and asks you.
- Tasks whose likely files overlap **never run at the same time**, not even
  a plan beside an implementation. Up to three non-overlapping tasks run in
  parallel.
- One plain command per Bash call, so the permission allowlist matches and
  nobody waits on a prompt you never saw.
- A **retro** every 15 finished tasks. No new task starts while one is due.
- Never fabricate expected results: derive them from the source.

## Security

The team runs unattended, so `.claude/settings.json` allows whole programs
(`git`, `make`, `python3`, your Jira CLI). An agent that may run `python3`
or `make` can run any code it writes. The deny list (force-push, `sudo`,
`git config`, editing the settings files) is a guard rail against mistakes,
not a security boundary: prefix rules cannot stop a determined interpreter.

- Run the team only on repositories and machines where that is acceptable,
  ideally inside Claude Code's sandbox, a container or a VM without
  personal credentials.
- Narrow the allow list where you can: specific make targets
  (`Bash(make test)`), specific scripts, specific git subcommands, and
  `WebFetch(domain:...)` for the sites you need.
- Treat downloaded samples as untrusted: agents read them, never run their
  code. Run Python that reads them with `python3 -I`.
- Keep the agent's Git and Jira credentials scoped to this one project.

## License

MIT. See [LICENSE](LICENSE).
