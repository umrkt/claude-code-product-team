# Jira via twg

Write every command literally as shown (absolute path and `--site` spelled
out, no shell variables) so it matches the project's permission rules and
runs without a prompt. If you use another Jira CLI, rewrite this table and
keep the rest of the rules.

```text
List board:   <TWG_PATH> jira workitem query --site <JIRA_SITE> --jql 'project = <JIRA_KEY> ORDER BY rank ASC' --limit 50
Read one:     <TWG_PATH> jira workitem get <KEY> --site <JIRA_SITE>
Transitions:  <TWG_PATH> jira workitem transition --id <KEY> --site <JIRA_SITE>
Move status:  <TWG_PATH> jira workitem transition --id <KEY> --transition-id '<status name>' --site <JIRA_SITE>
Comment:      <TWG_PATH> jira workitem comment create --issue-id <KEY> --body '<one-line html>' --site <JIRA_SITE>
Create task:  <TWG_PATH> jira workitem create --site <JIRA_SITE> --space <JIRA_KEY> --type Task --summary '<s>' --description '<one-line html>'
Link:         <TWG_PATH> jira workitem link workitem --id <BLOCKER> --target-id <BLOCKED> --link-type-id 'Blocks' --site <JIRA_SITE>
```

Every command must be a single line: a newline inside a quoted argument makes
the user approve the command by hand. So write descriptions and comments as
one-line HTML, e.g.
`<h2>Goal</h2><p>…</p><h2>Acceptance criteria</h2><ol><li>…</li></ol>`.
Use `<strong>`, not `<b>`.
Never use shell variables, loops or pipes around the CLI.

Every comment starts with the role whose work it reports, in the form
`<p><strong>[Role · round]</strong> …</p>`: `[PO]` for your own decisions,
scope notes and releases; `[Developer · fix 2/5]`, `[Reviewer · round 3]`,
`[QA · round 1]` when relaying that worker's hand-back; `[User]` for a
decision the user made. All comments post under the user's Jira account,
so this prefix is the only way to tell who said what.

Put text arguments in single quotes so backticks and `$` stay literal. Inside
the text, avoid single quotes (rephrase, or use `’`).

Project `<JIRA_KEY>`, board <JIRA_BOARD>. Status names: `To Do`,
`In Development`, `Review`, `QA`, `Waiting For Release`, `Done`. Any status
can move to any status.

A task description holds: goal, acceptance criteria (numbered, checkable,
at least one of them a negative path: what must fail or stay unresolved),
and a "Likely files" section (required; used to decide what can run in
parallel).

## Errors

- Never guess a status from memory; read it.
- On an auth error (expired or missing login), stop and tell the user to log
  in themselves. Never run login or setup yourself.
- On any other command error, read the CLI's help for that command once and
  retry with the corrected shape; if it still fails, stop and report the
  exact error.
