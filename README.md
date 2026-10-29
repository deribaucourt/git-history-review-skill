# Git History Review

A portable agent skill for reviewing outgoing Git commits before a push or pull request. It checks the history and its messages against local project conventions and broadly useful commit-quality practices. It is read-only: it reports findings but does not edit or publish history.

## Repository contents

- [`skills/git-history-review/SKILL.md`](skills/git-history-review/SKILL.md) — the installable skill.
- [`tests/`](tests/README.md) — manual PASS and FINDINGS patch fixtures with expected outcomes.
- [`docs/superpowers/specs/`](docs/superpowers/specs/) — the approved design.

## Use the skill

Copy the `skills/git-history-review/` directory into the skills directory used by your coding assistant, keeping `SKILL.md` at the root of that directory. Invoke `git-history-review` for a read-only review of the commits intended for the next push or pull request.

The skill confirms the target and outgoing range before reviewing. It samples a small, explicit set of project guidance and recent target-branch commit messages to learn local conventions; it does not search the repository or full history recursively.

## Manual regression cases

Use the patch fixtures in [`tests/`](tests/README.md) as synthetic outgoing histories. Review each patch with the skill and compare the reported gate and finding occurrences with the companion expected-outcome note. The fixtures are demonstrations and manual regression inputs, not an automated scorer.

## Scope

The skill evaluates commit history and commit messages only. It does not review implementation correctness, amend or rebase commits, commit changes, or push branches. Any history rewrite must be handled by an authorized contributor.
