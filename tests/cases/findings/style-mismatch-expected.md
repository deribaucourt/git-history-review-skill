# Expected outcome: FINDINGS

## Review context

- Target branch: `main`.
- Outgoing range: the commit in `style-mismatch.patch`.
- Worktree: clean; the outgoing commit is unpublished.
- The repository's root `CONTRIBUTING.md` requires subjects in the form `TASK-<number>: <imperative summary>`, with a maximum subject width of 60 columns. It also requires a `Change-Id: I<hex>` footer and a `Signed-off-by: Name <email>` trailer.
- Three examples from the ten newest target-branch commits are `TASK-214: add response mapping`, `TASK-210: handle empty payload`, and `TASK-205: clarify adapter errors`; each includes both required trailers.

## Expected findings

1. The subject does not include the required `TASK-<number>:` prefix.
2. The subject exceeds the required 60-column width.
3. The commit message omits the required `Change-Id` footer.
4. The commit message omits the required `Signed-off-by` trailer.

## Expected gate

`FINDINGS`
