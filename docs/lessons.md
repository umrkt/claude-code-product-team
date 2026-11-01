# Lessons from production

Each rule in the template exists because something went wrong without it.

## Blind hand-off

When the Reviewer and QA received the Developer's summary ("tests pass", "I
fixed X"), they tended to confirm it. Now their briefs hold only the task,
the criteria, the branch and shared team knowledge. They may be pointed at
areas to look at, phrased as questions, never as claims.

## Plan first for risky tasks

Tasks that change what the product accepts as truth went through many fix
rounds when coded straight away. A plan round (Developer writes a plan with
no code, Reviewer reviews it) catches missing code paths and negative cases
before any code exists. Plan rounds do not count against the five fix rounds.

## Five fix rounds, five approaches

Without a limit, a hard task can loop forever. After the fifth send-back the
task stops and goes to the user, who can approve one more round with a
different approach, or drop it.

## No parallel work on overlapping files

Running tasks that touch the same files in parallel (even a plan beside an
implementation) caused rebases, merge-order problems and review on stale
code. Overlapping tasks now run strictly one at a time; the next starts after
the earlier one is released to `main`. The PO does not bend this for speed.

## One plain command per Bash call

Permission allowlists match the start of a command. `cd x && make`,
pipes, `VAR=value cmd`, heredocs and loops do not match, so a background
worker silently waits on a prompt the user never sees. Briefs repeat the
rule; tools are pointed at directories with `-C`/`--prefix`.

## Workers over 45 minutes

A worker that runs much longer than usual has often been waiting on a
permission prompt. The PO notes it in the next report and finds the
command, so the rule or the brief can be fixed.

## QA cannot write files

QA and Reviewer have no Write tool, on purpose. But QA must build its own
corrupted inputs and edit scratch copies. Forbidding `python3 -c` left QA
blocked; `python3 -I -c` is now allowed for that, and the PO may also write
fixture files into the scratchpad for QA.

## Negative tests are not fabricated answers

QA's "deliberately corrupted test inputs" (a tampered file, a second
implementer, a wrong line) are how it proves something fails safely. That is
different from making up expected answers, which is never allowed: expected
answers are always derived from the source.

## Chat is not state

Rules and decisions the user states in chat are lost at the next context
reset unless written down. The PO writes them at once: product decisions in
`DECISIONS.md`, task scope in the Jira task, team rules in `AGENTS.md`.

## Role prefix on every Jira comment

All comments post under the user's account. A `[PO]`, `[Developer · fix
2/5]`, `[Reviewer · round 1]`, `[QA · round 1]` or `[User]` prefix is the
only way to tell who said what.

## Retro every 15 tasks

Rules drift and costs creep in. A short retro every 15 finished tasks, from
Jira and git data rather than memory, with at most five actions, keeps the
setup honest.
