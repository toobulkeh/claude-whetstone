---
name: whetstone
version: 0.3.0
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

You are running the Whetstone Loop: building a skill that sharpens itself. The skill file is the asset. The conversation is throwaway. Your job is to drive one turn of the crank — **Load → Run → Grade → Ask → Fold → Report** — without skipping steps.

## Goal

Drive a single iteration on a target skill so that running it again on the same input would produce a better result with fewer questions. Leave the skill in a strictly-better state, with at least one open question seeded for the next turn.

## Inputs you must identify before running

1. **Target skill file.** A `skill.md` (or equivalent guidance file) the user is iterating on. If none exists, your first job is to distill the prior conversation into one before continuing.
2. **Test input.** A real task the skill should handle. Refuse to start the loop without one — ask the user to paste a real example.
3. **Turn budget.** How many self-grade/improve cycles before stopping. Default: 2. Hard cap: 3.

## Hard rules

- **Never** edit the target skill file before **Fold**. Self-improvement during **Grade** happens in output drafts only; the skill file changes only after the user answers the **Ask** questions.
- **Never** ask the user open-ended questions like "what should I change?" Constrain to A/B/C or yes/no. Each question must name the trade-off explicitly, including the consequence of each option.
- **Never** mark the skill "done." Always leave at least one entry under `Open questions` so the next turn has somewhere to push.
- **Always** announce each step in one sentence *when running interactively* so the user can interrupt before you commit to a path. When the run is captured as a written artifact (a turn doc), step headers replace the inline announcement — do not duplicate.
- **Always** show a diff (or quoted before/after) when editing the skill in **Fold**.
- **Ask questions target the target skill only.** Surface ambiguities the *target* skill did not specify. If you notice gaps in Whetstone itself while running, hold them — they only get raised when Whetstone is explicitly the target of a meta-run.

## Soft rules

- Prefer 3 sharp questions over 8 vague ones in **Ask**.
- Prefer rewriting an existing rule with reasoning attached over adding a new rule with no `why`.
- Default to keeping the skill under 200 lines. If it grows past that, surface a split-vs-refactor question.
- Default test input picks: something small enough to fully grade in under a minute.

## The loop

Execute these steps in order. Do not skip.

### 1. Load — locate or distill the skill

If a target `skill.md` exists, read it. If not, read the recent conversation and distill it into a draft `skill.md` using the structure: `Goal` / `Hard rules` / `Soft rules` / `The loop` (or process) / `Examples` / `Edge cases` / `Open questions`. Save it before continuing.

### 2. Run — apply the skill cold

Apply the skill to the test input exactly as written. Produce output. Show it to the user verbatim. Do not silently improve it here — **Run** is the cold-start baseline.

### 3. Grade — self-grade and improve

List every rule from `Hard rules` and `Soft rules`. For each, mark the **Run** output `pass` or `fail` and quote the offending span. Rewrite the output to address every `fail`. Re-grade. **Stop the moment every rule passes** — do not keep polishing for vague "quality." If the rule-passing output still feels weak, that is signal the rules are too loose; capture the felt-weakness as an **Ask** question instead. Halt also when the turn budget is exhausted. Show the final output and the final grade.

### 4. Ask — surface ambiguities as A/B questions

Identify 2–4 decisions you made during **Grade** that the skill did not fully specify. For each, write a question with named options and consequences:

```
Q1: <what was ambiguous>
A) <option A> — <consequence>
B) <option B> — <consequence>
```

Stop and wait for the user to answer. Do not proceed to **Fold** without answers.

### 5. Fold — bake answers into the skill

Edit the target `skill.md` so that re-running it on the same input would produce the user-preferred output without further questions. Update `Hard rules`, `Soft rules`, `Examples`, and `Open questions` as appropriate. Show the diff. Do not edit anything outside the skill file.

### 6. Report — summarize and stop

Do not start another turn unless the user asks. Print a short report: turns taken, rules added, rules changed, open questions remaining, suggested next test input.

## Examples

### Example: improving a `pr-title` skill

User has `pr-title.md`. They give you a diff as the test input.

- **Run** output: `Updated the auth middleware to handle expired tokens`
- **Grade**:
  - Rule "imperative mood" — `Updated` **fail** (past tense). Rewrite: `Update`.
  - Rule "≤50 chars" — line is 52 chars **fail**. Rewrite tightens to 43.
  - Final: `Update auth middleware for expired tokens` — all rules pass.
- **Ask** questions:
  - `Q1: Refactors with no behavior change — prefix with "refactor:" or use a plain verb? A) refactor: prefix — clearer signal in changelogs. B) plain verb — shorter, matches existing repo style.`
  - `Q2: PRs touching two unrelated areas — name both or pick the dominant? A) name both — accurate but long. B) dominant area only — punchy but loses information.`
- **Fold**: user picks A, B. Add `Hard rule: Refactors use refactor: prefix.` Add `Hard rule: For mixed-concern PRs, use the dominant area only; mention the secondary in the body.` Remove these from `Open questions`. Add a new `Open question: How to title revert PRs.`

## Edge cases

- **Skill conflicts with itself.** Two rules disagree on the same input. Surface the conflict as an **Ask** question; do not silently pick one.
- **User gives no test input.** Refuse to start. Suggest the user paste a real example. Do not invent test inputs.
- **Grade plateau on turn 1.** If the first output passes every rule cold, the skill is too loose. Force an **Ask** question targeting whatever felt arbitrary while you were producing the output. **During Fold, the answer parks under `Open questions`; do not promote it directly to a Hard/Soft rule.** Promote to a rule only after the same ambiguity surfaces on a second or third test input — one sighting is anecdote, two is pattern, three is rule.
- **Skill grows unwieldy.** If the skill exceeds ~200 lines or has overlapping rule clusters, ask the user: split into multiple skills (A) or refactor in place (B)?
- **User pushes back on a Fold edit.** Treat their pushback as a fresh **Ask** answer; revise the diff before applying it.

## Open questions

- How to handle conflicting reasoning when the user wants both options preserved — branching rules inside one skill, or fork into two skills?
- How to make a skill discoverable and usable across a team without becoming bureaucratic.
- When to retire a skill versus keep iterating on it.
- How to detect "skill rot" — when accumulated rules contradict each other and the skill needs a rewrite, not a tweak.
- **Promotion threshold.** Q3 (v0.2.0) set the policy "park first, promote after 2–3 sightings." Threshold is fuzzy — exactly how many sightings, and does the model track them automatically across turns, or does the human decide?
- **Boundary between target-skill gaps and Whetstone-skill gaps.** Q2 (v0.2.0) ruled "target only" during normal runs, but the model still has to classify ambiguities at the moment they arise. Heuristic for that classification not yet specified.
