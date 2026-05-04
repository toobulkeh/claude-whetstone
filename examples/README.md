# Examples

Two runs captured here:

- [`pr-title/`](./pr-title/) — Whetstone applied to a `pr-title` skill on a single-concern auth-middleware diff. Walks through **Load → Run → Grade → Ask**, ends blocked on user A/B answers.
- [`whetstone-self/`](./whetstone-self/) — the meta-loop: Whetstone applied to its own `skill.md`, using the `pr-title` run as the test input. Surfaces four ambiguities in Whetstone itself; the **Fold** for that run actually landed (v0.2.0).

The `pr-title/` run stops at **Fold** because that step requires the human's A/B answers. The point of the artifacts is to make the loop legible: every step, every grade, every question is on disk and reviewable.

The six steps: **Load** (locate or distill the target skill), **Run** (apply cold), **Grade** (self-grade against the skill's own rules and rewrite), **Ask** (surface ambiguities as A/B questions), **Fold** (bake user answers into the skill file), **Report** (summarize and stop).
