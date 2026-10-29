---
name: git-history-review
description: Review outgoing Git commits before a push for project message conventions, atomicity, clean history, and reviewer-friendly commit narratives; report PASS, FINDINGS, or BLOCKED without modifying history.
---

# Git History Review

## Purpose and boundaries

Review the proposed outgoing commit range before it is pushed or included in a pull request. This is a history and commit-message review, not a source-code correctness, security, or test review.

Operate read-only. Do not edit files, stage changes, amend or rebase commits, create commits, push, or delegate changes. Report findings and minimal corrections for the implementation owner.

## 1. Confirm the review range

1. Inspect the current branch and worktree state with `git status --short --branch` and `git branch -vv`.
2. Establish the intended target from the user's explicit request or a verified upstream/tracking relationship. Do not guess a target from the branch name or from a missing upstream.
3. Confirm the outgoing range and publication state before reviewing. Use the confirmed target and `git log --graph --oneline <target>..HEAD` to identify the commits under review. Treat uncommitted changes as outside the commit range.
4. If the target, base, outgoing range, or relevant publication state cannot be established reliably, return `BLOCKED` with the exact missing information. Do not review a guessed range or recommend rewriting published history without authorization.

## 2. Learn the project's message style quickly

Use only this bounded discovery pass; do not recursively search the repository for instruction files or scan its full Git history.

1. Check these repository-root resources if present:
   - `AGENTS.md`
   - `CONTRIBUTING.md`
   - A commit-policy document only when one of the two files above directly links to it
   - `.gitmessage`
   - `commitlint.config.*` and `.commitlintrc*`
2. Inspect at most the ten newest commits on the confirmed target branch:

   ```sh
   git log -10 --format='%h %s%n%b' <confirmed-target>
   ```

3. Note the recurring subject format, capitalization and tense, issue identifiers, scopes, required trailers (including sign-off or change identifiers), and any stated or consistently observed subject/body column limits. Distinguish ordinary commits from merge messages when comparing styles.
4. Treat written project guidance and enforced configuration as requirements. Treat repeated patterns in recent commits as evidence of convention, not as an automatic rule. A single example, absent configuration, or sparse history is not enough to invent a required prefix, tracker key, trailer, or line-length limit.
5. If the evidence is mixed, record the disagreement and its source. Use general best practices below when no reliable project-specific requirement is available.

## 3. Review every outgoing commit

Review the complete confirmed range, not just its newest commit. Compare each commit's full subject, body, trailers, and change scope with all applicable written requirements, reliable project conventions, and the general practices below. Use the diff or file summary only as needed to judge atomicity and size; do not turn this into a source-code review.

### Commit-message quality

- Titles should be concise yet descriptive: identify the change clearly without filler or a vague "update" / "fix" alone.
- The subject should describe what changed. The body should explain the context and why the change is needed, rather than narrating implementation steps or how the author performed the work.
- Follow explicit local title formats and required issue references, scopes, sign-offs, `Change-Id` values, trailers, and line-width policies.
- Keep bodies focused. Repetition, generic explanations, unnecessary process narration, and long AI-style summaries are red flags; do not infer authorship from verbosity alone.
- A title or body that says it does "A and B" is a signal to check for mixed scope, not an automatic finding if both describe one coherent logical change.
- An undocumented change, not reflected in the commit message, is considered a red flag.

### Atomicity and commit size

- Keep each commit to one logical change. Multiple unrelated fixes should not be bundled together.
- Separate preparatory refactors or scaffolding from feature implementation. Implement larger features in small, understandable logical iterations.
- Add very large tests separately from the functional implementation; keep focused tests with their logical change when they remain small and cohesive.
- Treat a commit larger than **Small** as a red flag. Judge size qualitatively from the diff's scope, complexity, and number of concerns, not a universal line-count threshold. Explain when a larger indivisible change has a clear justification.
- Inspect the commit's actual scope; do not decide atomicity from the subject alone.
- A change unrelated to the current logical scope or commit message is a red flag.

### Clean history and narrative order

- A pull-request history should not contain "fixes on top": fixup, squash, WIP, or corrective commits whose purpose is to repair an earlier commit in the same series. Prefer constructing the final solution iteratively as clean, reviewable commits.
- A rebase/squash is the recommended way to produce a coherent final series when appropriate. This reviewer does not perform it; rewriting a published/shared branch requires authorization and coordination.
- Order commits so a reviewer can follow the narrative: prerequisites and justified preparation first, then small implementation steps, then separately scoped large tests or supporting changes. Keep dependencies understandable and avoid unrelated jumps.

## 4. Decide the gate

Check every commit against every applicable rule. Report every confirmed violation occurrence, even when another finding already blocks the range. If a single commit has two distinct atomicity problems or repeats a violation, report each occurrence separately; do not collapse them into one summary.

- `PASS`: the target and outgoing range are known, every commit was checked, and no violations were found.
- `FINDINGS`: the range is known and one or more violations were confirmed.
- `BLOCKED`: missing or conflicting target, range, or policy context prevents a reliable verdict. Include all confirmed findings that can still be established.

For each finding, cite the commit, the exact applicable rule or clearly identified convention, the evidence for that occurrence, and the minimal required correction. Do not report a personal preference as a project rule.

## Result format

```text
## Git Strategy Review
Result: PASS | FINDINGS | BLOCKED
Branch: <name or unknown>
Upstream/target: <name or unknown>
Outgoing range: <base..HEAD or unavailable>
Blocker: <reason, when BLOCKED>
Findings:
# Repeat a finding block for each violation occurrence.
- Commit: <short hash>
  Rule: <source and exact rule, or identified general best practice>
  Evidence: <commit or diff evidence identifying this occurrence>
  Required correction: <minimal action>
```

For `PASS`, use `Findings: None`. For `FINDINGS`, include one entry for every confirmed violation occurrence, including repeated instances of the same rule on the same commit. For `BLOCKED`, state what is missing and list any findings that remain reliable.
