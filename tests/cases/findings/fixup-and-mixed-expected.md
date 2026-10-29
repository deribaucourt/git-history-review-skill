# Expected outcome: FINDINGS

## Review context

- Target branch: `main`.
- Outgoing range: all commits in `fixup-and-mixed-history.patch`.
- Worktree: clean; the outgoing commits are unpublished.
- Project guidance: no additional atomicity policy, issue key, sign-off, or change identifier requirement.
- Recent target-branch messages commonly use concise `<type>: <imperative summary>` subjects.

## Expected findings

1. The second commit is a `fixup!` correction on top of the first commit in the same outgoing history. The final pull-request history should be constructed as a clean series instead of retaining a corrective commit.
2. The third commit changes both request parsing and logging, which are unrelated concerns. Split the changes into atomic commits. The subject's use of "and" is supporting evidence for this mixed scope, not a separate finding.

## Expected gate

`FINDINGS`
