# Turn 1 — Whetstone applied to `pr-title` skill

Target skill: `examples/pr-title/skill.md` (v0.1.0)
Test input: `examples/pr-title/test-input.md`
Turn budget: 2

## Phase 1 — Locate

Found `examples/pr-title/skill.md`. Loaded its rules. Test input is a single-concern diff (clock-skew tolerance on JWT verify).

## Phase 2 — Run cold

> `Updated the auth middleware to accept tokens with a small amount of clock drift.`

## Phase 3 — Self-grade and improve

Pass through 1:

| Rule | Result | Note |
|---|---|---|
| Imperative mood | **fail** | "Updated" is past tense. |
| ≤50 chars | **fail** | 79 chars. |
| No trailing period | **fail** | Trailing `.`. |
| Lead with the verb | pass | "Updated" leads. |
| Smallest accurate scope | partial | "auth middleware" is good; "small amount of clock drift" is verbose. |

Rewrite 1:

> `Allow JWT clock skew of 30s in auth middleware`

Pass through 2:

| Rule | Result | Note |
|---|---|---|
| Imperative mood | pass | "Allow". |
| ≤50 chars | pass | 47 chars. |
| No trailing period | pass | — |
| Lead with the verb | pass | "Allow". |
| Smallest accurate scope | pass | Names module + behavior precisely. |

All rules pass. Turn budget not exhausted, but no `fail` remaining → stop grading.

**Final output:** `Allow JWT clock skew of 30s in auth middleware`

## Phase 4 — Ambiguities (A/B questions for the user)

While producing the rewrite, three decisions were not specified by the skill:

```
Q1: Bug-fix vs. feature framing.
   The diff fixes intermittent 401s but is implemented as "allow X."
   Should the title surface the *symptom fixed* or the *behavior added*?
   A) Symptom — `Fix intermittent 401s on clock drift`
   B) Behavior — `Allow JWT clock skew of 30s in auth middleware`
   Trade-off: A is more reviewer-useful; B is more accurate to the diff.

Q2: Numeric specifics in the title.
   The diff hard-codes 30s. Should the title include the number?
   A) Yes — `... 30s ...` is precise but ages poorly if tuned later.
   B) No — generic phrasing is durable but loses info.

Q3: Module name format.
   Use code-style (`auth/middleware`) or prose (`auth middleware`)?
   A) Code-style — matches file paths, scannable in long lists.
   B) Prose — reads like English in changelogs.
```

## Phase 5 — Fold back

Blocked. Waiting on user answers to Q1–Q3 before editing `skill.md`.

## Phase 6 — Report (preliminary)

- Turns taken: 1 (one rewrite, all rules passed)
- Rules added: 0 yet (pending Phase 5)
- Open questions still on the skill: 2 of 2 (refactors, mixed-concern PRs) — both untouched, but Q1–Q3 above are candidates to add or replace them.
- Suggested next test input: a refactor-only diff, to force a decision on the first existing open question.
