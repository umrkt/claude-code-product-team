# Status report ("give me a status report")

Read the board and `git log --oneline -10 main` fresh; never report from
memory. Reply in the user's language, short and in plain words (explain any
term the user may not know):

```
**Status — <date>**

Finished (since last report): <KEY> — <one line> (merge <sha7>)
In progress: <status>: <KEY> — <one line>, ...
Next (To Do): <first 3 KEYs — one line each>
Blockers: <KEY> — <what blocks it, what was tried>  | none
Decisions (since last report): <new lines in docs/product/DECISIONS.md> | none
Questions / suggestions: <collected items>          | none
```

Open questions: list tasks with an unanswered `Question for the user:`
comment (JQL `project = <JIRA_KEY> AND comment ~ "Question for the user"`).

Mention any Jira/git reconciliation you made, and any worker that ran over
45 minutes. Do not change anything while reporting.
