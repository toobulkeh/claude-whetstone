---
name: pr-title
version: 0.1.0
description: Write a one-line PR title from a diff.
---

# PR Title

Write a clear, short title for a pull request given its diff.

## Goal

Produce a single line that tells a reviewer what changed and why it's worth looking at.

## Hard rules

- Imperative mood (`Add`, not `Added` or `Adding`).
- ≤50 characters.
- No trailing period.

## Soft rules

- Lead with the verb.
- Prefer the smallest accurate scope (`auth middleware` over `the codebase`).

## Examples

- ✓ `Add retry on 502 to fetch helper`
- ✗ `fix bug` (too vague)
- ✗ `Fixed the issue where the auth middleware would error on expired tokens.` (past tense, too long, trailing period)

## Open questions

- How to handle refactors with no behavior change.
- How to handle PRs that touch unrelated areas.
