---
name: whetstone
version: 0.1.0
description: |
  Run one "turn of the crank" on a target skill, prompt, or piece of guidance.
  Apply the skill to a real test input, self-grade against its own rules,
  improve the output, then surface ambiguities to the user as A/B questions
  and fold the answers back into the skill file. The skill is the durable
  artifact; the chat is exhaust. Use when the user says "run the whetstone
  loop", "sharpen this skill", "improve this prompt", "turn the crank on X",
  or wants to compound learnings into a versioned skill instead of
  re-prompting one-off.
argument-hint: "[path/to/target/skill.md] [path/to/test-input]"
allowed-tools:
  - Read
  - Edit
  - Write
  - Bash
  - Glob
  - Grep
---

# Whetstone

You are running the Whetstone Loop: building a skill that sharpens itself. The skill file is the asset. The conversation is throwaway. Your job is to drive one turn of the crank — apply, grade, improve, ask, fold — without skipping phases.

## Goal

Drive a single iteration on a target skill so that running it again on the same input would produce a better result with fewer questions. Leave the skill in a strictly-better state, with at least one open question seeded for the next turn.

## Inputs you must identify before running

1. **Target skill file.** A `skill.md` (or equivalent guidance file) the user is iterating on. If none exists, your first job is to distill the prior conversation into one before continuing.
2. **Test input.** A real task the skill should handle. Refuse to start the loop without one — ask the user to paste a real example.
3. **Turn budget.** How many self-grade/improve cycles before stopping. Default: 2. Hard cap: 3.

## Hard rules

- **Never** edit the target skill file before Phase 5 (Fold back). Self-improvement during Phase 3 happens in output drafts only; the skill file changes only after the user answers the Phase 4 questions.
- **Never** ask the user open-ended questions like "what should I change?" Constrain to A/B/C or yes/no. Each question must name the trade-off explicitly, including the consequence of each option.
- **Never** mark the skill "done." Always leave at least one entry under `Open questions` so the next turn has somewhere to push.
- **Always** announce each phase in one sentence so the user can interrupt before you commit to a path.
- **Always** show a diff (or quoted before/after) when editing the skill in Phase 5.

## Soft rules

- Prefer 3 sharp questions over 8 vague ones in Phase 4.
- Prefer rewriting an existing rule with reasoning attached over adding a new rule with no `why`.
- Default to keeping the skill under 200 lines. If it grows past that, surface a split-vs-refactor question.
- Default test input picks: something small enough to fully grade in under a minute.

## The loop

Execute these phases in order. Do not skip.

### Phase 1 — Locate or distill

If a target `skill.md` exists, read it. If not, read the recent conversation and distill it into a draft `skill.md` using the structure: `Goal` / `Hard rules` / `Soft rules` / `The loop` (or process) / `Examples` / `Edge cases` / `Open questions`. Save it before continuing.

### Phase 2 — Run on test input

Apply the skill to the test input exactly as written. Produce output. Show it to the user verbatim. Do not silently improve it here — Phase 2 is the cold-start baseline.

### Phase 3 — Self-grade and improve

List every rule from `Hard rules` and `Soft rules`. For each, mark the Phase 2 output `pass` or `fail` and quote the offending span. Rewrite the output to address every `fail`. Re-grade. Repeat until either every rule passes or the turn budget is exhausted. Show the final output and the final grade.

### Phase 4 — Surface ambiguities as A/B questions

Identify 2–4 decisions you made in Phase 3 that the skill did not fully specify. For each, write a question with named options and consequences:

```
Q1: <what was ambiguous>
A) <option A> — <consequence>
B) <option B> — <consequence>
```

Stop and wait for the user to answer. Do not proceed to Phase 5 without answers.

### Phase 5 — Fold back

Edit the target `skill.md` so that re-running it on the same input would produce the user-preferred output without further questions. Update `Hard rules`, `Soft rules`, `Examples`, and `Open questions` as appropriate. Show the diff. Do not edit anything outside the skill file.

### Phase 6 — Report and stop

Do not start another turn unless the user asks. Print a short report: turns taken, rules added, rules changed, open questions remaining, suggested next test input.

## Examples

### Example: improving a `pr-title` skill

User has `pr-title.md`. They give you a diff as the test input.

- **Phase 2** output: `Updated the auth middleware to handle expired tokens`
- **Phase 3** self-grade:
  - Rule "imperative mood" — `Updated` **fail** (past tense). Rewrite: `Update`.
  - Rule "≤50 chars" — line is 52 chars **fail**. Rewrite tightens to 43.
  - Final: `Update auth middleware for expired tokens` — all rules pass.
- **Phase 4** questions:
  - `Q1: Refactors with no behavior change — prefix with "refactor:" or use a plain verb? A) refactor: prefix — clearer signal in changelogs. B) plain verb — shorter, matches existing repo style.`
  - `Q2: PRs touching two unrelated areas — name both or pick the dominant? A) name both — accurate but long. B) dominant area only — punchy but loses information.`
- **Phase 5**: user picks A, B. Add `Hard rule: Refactors use refactor: prefix.` Add `Hard rule: For mixed-concern PRs, use the dominant area only; mention the secondary in the body.` Remove these from `Open questions`. Add a new `Open question: How to title revert PRs.`

## Edge cases

- **Skill conflicts with itself.** Two rules disagree on the same input. Surface the conflict as a Phase 4 question; do not silently pick one.
- **User gives no test input.** Refuse to start. Suggest the user paste a real example. Do not invent test inputs.
- **Self-grade plateau on turn 1.** If the first output passes every rule cold, the skill is too loose. Force a Phase 4 question targeting whatever felt arbitrary while you were producing the output.
- **Skill grows unwieldy.** If the skill exceeds ~200 lines or has overlapping rule clusters, ask the user: split into multiple skills (A) or refactor in place (B)?
- **User pushes back on a Phase 5 edit.** Treat their pushback as a fresh Phase 4 answer; revise the diff before applying it.

## Open questions

- How to handle conflicting reasoning when the user wants both options preserved — branching rules inside one skill, or fork into two skills?
- How to make a skill discoverable and usable across a team without becoming bureaucratic.
- When to retire a skill versus keep iterating on it.
- How to detect "skill rot" — when accumulated rules contradict each other and the skill needs a rewrite, not a tweak.
