# The Whetstone Loop

> A technique for building skills that sharpen themselves.

Stop optimizing the deliverable. Optimize the system that produces deliverables.

You build a small piece of guidance (a `skill.md`, a prompt, a checklist), then turn a crank: have the model use it, grade itself, improve, and let you sand off the rough edges with targeted feedback. The skill becomes the source of truth — every future "turn" is run against it, and every learning goes back into it.

## Why it works

Most prompting is one-shot: you write, you run, you tweak in the moment, you lose the lesson. The Whetstone Loop captures the lesson in a durable artifact. The artifact compounds — every turn either confirms the rules or refines them. Over a week, the skill is dramatically better than anything you could have written up front.

You're not building the product. You're building the factory.

## The loop

1. **Reason out loud.** Have a normal conversation with the model about the problem. Don't try to be clever — just explain why, how, and what trade-offs you care about.
2. **Distill.** Ask the model to read the conversation and write a `skill.md` capturing the reasoning, decisions, and rules. Save it. This is now the source of truth.
3. **Turn the crank.** Run the skill on a real input. Have the model:
   - Produce output.
   - Grade its own output against the skill.
   - Improve based on its own grade.
   - Repeat 1–3 times until the self-grade plateaus.
4. **Targeted feedback.** Read the final output. Ask the model to ask *you* pointed questions — yes/no or A/B/C choices — about the trade-offs it made or the ambiguities it hit. Add freeform notes where the multiple-choice frame is wrong.
5. **Fold back.** Have the model rewrite the skill with your feedback baked in. New rules, new examples, new edge cases.
6. **Always edit the skill.** Future runs go through the skill. New learnings update the skill. The chat is throwaway; the artifact is the asset.

## Why pointed questions beat open-ended ones

"What should I change?" produces sycophancy and noise. Constraining the model to A/B/C surfaces the actual decision points, lets you decide fast, and forces the model to articulate the trade-off explicitly. It also exposes when the model isn't sure — the questions it asks tell you where the skill is thin.

## What belongs in a skill.md

- **Goal.** One sentence. What does running this skill produce?
- **Hard rules.** Things that are always true. (`Never include the ticket prefix.`)
- **Soft rules.** Defaults and preferences. (`Prefer imperative mood unless the convention says otherwise.`)
- **Examples with reasoning.** Not just ✓/✗ — *why* the good one is good.
- **Known edge cases.** And how the skill resolves them.
- **Open questions.** Anti-pattern: pretending the skill is finished. Mark uncertainty so future turns know where to push.

## When to use it

- Anything you'll do more than three times.
- Anything where the *quality bar* matters more than speed.
- Anything where you've caught yourself giving the same correction twice.

## When not to use it

- One-shot tasks.
- Throwaway exploration where the answer is the point.
- Work where requirements aren't yet stable enough to encode — keep talking first.

## Open frontier

- **Split vs. improve.** When two reasoning paths conflict inside one skill, do you fork it into two skills, or thread the conflict into the existing one with branching rules? My current rule of thumb: split when the two paths have different *audiences* or different *success criteria*; otherwise improve in place with explicit `if/else` rules.
- **Democratization.** A skill is only valuable if the team uses it. How do these compound across a team — shared repo? PR review on the skill itself? Skill registries with their own changelogs? This is unsolved.

## The bet

This is the new way of working: explicit micromanagement on steroids. Not micromanaging the model turn by turn — micromanaging the *rules the model runs by*, captured in a file you can edit, share, and version. The skill is the artifact. The conversations that produced it are exhaust.
