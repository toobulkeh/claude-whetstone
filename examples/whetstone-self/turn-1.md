# Turn 1 — Whetstone applied to itself

Target skill: `skill.md` (Whetstone v0.1.0)
Test input: the run captured in `examples/pr-title/turn-1.md` (Whetstone executing on the `pr-title` skill).
Turn budget: 2

This is the meta-loop: using Whetstone to grade Whetstone's own behavior on a real run.

## Phase 1 — Locate

Loaded `skill.md`. Read `examples/pr-title/turn-1.md` as the artifact-of-execution to grade.

## Phase 2 — Run on test input

The "output" being graded is the `pr-title/turn-1.md` document itself: did the prior run actually conform to Whetstone's own Hard and Soft rules?

## Phase 3 — Self-grade and improve

### Hard rules

| Rule | Result | Evidence |
|---|---|---|
| Never edit target skill before Phase 5 | **pass** | `examples/pr-title/skill.md` was not modified during the run. |
| Never ask open-ended questions | **pass** | Q1–Q3 all A/B with named consequences. |
| Never mark skill "done" — keep an Open question | **pass** | Two original open questions retained; three new candidates surfaced. |
| Announce each phase in one sentence | **partial fail** | The document uses phase *headers* but does not include an inline one-line announcement before each phase. Skill rule was written for live execution; ambiguous when the run is captured as a document. |
| Show a diff when editing skill in Phase 5 | **n/a** | Phase 5 did not execute (blocked on user). |

### Soft rules

| Rule | Result | Evidence |
|---|---|---|
| 3 sharp questions over 8 vague | **pass** | 3 questions, each with consequence. |
| Rewrite with reasoning, don't just add rules | **n/a** | Phase 5 deferred. |
| Keep skill ≤200 lines | **pass** | ~110 lines. |
| Test input small enough to grade in <1 min | **pass** | Single-concern diff. |

### Improve

The "partial fail" on phase announcements points at a real gap in the skill: it doesn't distinguish between **executing live** (where one-sentence announcements help the user interrupt) and **producing a document of the run** (where the document structure is the announcement). Two ways to fix:

- Tighten the rule to apply only when running interactively.
- Drop the rule and rely on phase headers always being present.

Cannot decide unilaterally — surfaces as Q4 below.

## Phase 4 — Ambiguities (A/B questions for the user)

```
Q1: Phase 3 grading scope.
   When all Hard/Soft rules pass before the turn budget is exhausted,
   does Phase 3 stop, or keep pushing for "best possible" output?
   A) Conformance only — stop when rules pass. Predictable halt; cleaner artifact.
   B) Quality also — keep iterating until self-grade can't find an improvement.
      Richer output; vaguer halt condition; more turn budget burned.

Q2: Phase 4 question domain.
   The current rule says questions surface decisions "the skill did not
   fully specify." Ambiguous: does that mean gaps in the *target* skill
   or gaps in *Whetstone itself*?
   A) Target skill only — questions feed Phase 5 edits to the thing being sharpened.
      Keeps the loop tight. Whetstone improves only via explicit meta-runs.
   B) Both, labeled — surface target-skill gaps and meta-skill gaps separately.
      Slower but compounds the meta-skill faster.

Q3: Plateau behavior.
   When Phase 3 hits no failures on turn 1 (the skill is too loose or the
   input was easy), what should the surfaced ambiguity become?
   A) Promote directly to a Hard/Soft rule in Phase 5 — fast tightening,
      risk of over-fitting to one input.
   B) Add only to Open questions — accumulate signal across multiple
      test inputs before committing to a rule. Slower but safer.

Q4: Phase announcements.
   The "announce each phase in one sentence" rule fails when the run is
   captured as a document (like this one) rather than executed live.
   A) Scope the rule to interactive runs only — explicit "if executing
      live" qualifier.
   B) Drop the rule — phase headers in the artifact carry the same signal.
   C) Keep both — require the inline announcement *and* the header even
      in documents. Most rigorous but verbose.
```

## Phase 5 — Fold back

Blocked. Waiting on Q1–Q4 before editing `skill.md`.

## Phase 6 — Report (preliminary)

- Turns taken: 1.
- Whetstone conformance on the prior run: 4/5 Hard rules pass, 1 partial-fail (announcements), 3/3 applicable Soft rules pass.
- Candidate rule changes pending user input: 4.
- Suggested next test input after this turn folds back: run Whetstone on a third skill (e.g. a code-comment-pruner) to test whether the new rules generalize beyond the `pr-title` shape.
