# QA checks

Apply what fits the task. Report each check with the command and result.
Replace the placeholders with your project's real commands.

## Always

- `<BUILD_CMD>` and `<TEST_CMD>` in the worktree, and `<FORMAT_CHECK>` on
  the files the branch changes (must report nothing).
- Every other test suite the repository has.
- Try at least one negative path from the acceptance criteria (bad input,
  tampered file, wrong line, missing tool) and confirm it fails safely.

## Running the product

- `<RUN_PRODUCT>` on a scratch copy of each relevant sample. Never run on
  the repository's own test data in place.
- Compare main and branch on the same samples, so a change is measured, not
  guessed.
- Downloaded samples are untrusted: read them, never run their code, builds
  or tests.

## Per output type

Add one section per kind of output your product makes. For each, say what
"correct" means and how QA proves it, for example:

- **Expected answers.** Derive the answer yourself from the source before
  opening the answer file, then compare. Spot-check at least five facts per
  sample, including that cited lines really prove them.
- **Generated documents** (specs, reports). Validate with a standard
  validator; nothing unproven is invented.
- **Build inputs and packaging.** Check digests against the upstream
  publisher yourself, try a tampered input, assemble and run the result.
