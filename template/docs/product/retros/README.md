# Retrospectives

A retro looks back over a batch of finished tasks and decides what the team
changes. Results are written here; changed rules go where they belong
(`AGENTS.md`, the `production-line` skill, `DECISIONS.md`, Jira).

## When

- After every 15 tasks that reached `Done` since the previous retro.
- When a retro is due, the PO starts no new task. Running tasks finish
  first; then the PO prepares the data and holds the retro with the user.

Count: `project = <JIRA_KEY> AND status = Done AND resolved > "<previous retro date>"`.
Each retro file records the last task it covers, so the next count starts
after it.

## Steps

1. **Data (PO, before the meeting).** Read Jira and git, not memory. For
   each task in the batch: fix rounds, plan rounds, who found each defect
   (plan review, Review, QA, after release), worker durations over 45
   minutes, permission prompts the user reported, tasks stopped for the
   user. Also: MVP bar status (`GOAL.md`) and sample scores before/after.
2. **Questions.** The PO answers each question below from the data and
   gives a short view; the user adds theirs.
3. **Actions.** At most five, each with an owner, a place (file or Jira key)
   and a check for the next retro. The PO applies them right after the
   retro.
4. **Record.** Write `docs/product/retros/YYYY-MM-DD.md` from the template,
   commit it on `main` and push.

## Questions

### Results
1. Which tasks were finished, and what did each change for the user? How
   far is the MVP bar now?
2. Which planned tasks did not finish, and why?
3. Did any released change have to be fixed again later?

### Quality
4. How many defects did plan review, Review and QA find? Which of them would
   have reached `main` without that step?
5. Which tasks used many fix rounds (3 or more) or hit the five-round limit?
   What was the common cause: unclear criteria, a weak first approach, a
   rule that kept being bypassed?
6. Did a task change what counts as truth without a plan round? Should it
   have had one?
7. Were expected answers and scores always derived from source? Any doubt?

### Process
8. Did briefs give workers what they needed? What did a worker have to
   guess, and what did the PO forget to pass on?
9. Blind hand-off: did blind Review and QA find things the Developer did not
   describe? Is it worth its cost?
10. Did any rule get bent (parallel overlap, fix-round limit, approval)? Why,
    and should the rule change or be enforced?
11. Where did work wait: permission prompts, long workers, usage limits,
    blocked tasks, merge order?
12. Was the Jira board always a true picture? Did the user have to ask for
    something the board should have shown?

### Collaboration with the user
13. Which questions did the PO bring to the user that it should have decided
    itself, and which did it decide that were the user's to make?
14. Were reports clear, short and in plain language? What was confusing or
    misleading?
15. What did the user have to repeat or correct?

### Next batch
16. Keep: what worked and must stay?
17. Stop: what costs more than it gives?
18. Start: what should we try in the next batch?
19. Is the backlog order still right for the goal and the MVP bar?

## Template

```markdown
# Retro YYYY-MM-DD

Covers: <first KEY> … <last KEY> (<n> tasks Done since <previous retro>)

## Data
| Task | Fix rounds | Plan rounds | Found by plan review / Review / QA / later | Notes |
|---|---|---|---|---|

MVP bar: <status>. Permission prompts: <n>. Workers over 45 min: <list>.

## Answers
<question number: short answer, PO and user views>

## Actions
| # | Action | Owner | Where | Check at next retro |
|---|---|---|---|---|

## Applied
<commit or Jira key per action>
```
