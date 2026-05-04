# Examples

Two runs captured here:

- [`pr-title/`](./pr-title/) — Whetstone applied to a `pr-title` skill on a single-concern auth-middleware diff. Walks through Phases 1–4, ends blocked on user A/B answers.
- [`whetstone-self/`](./whetstone-self/) — the meta-loop: Whetstone applied to its own `skill.md`, using the `pr-title` run as the test input. Surfaces four ambiguities in Whetstone itself.

Both runs stop at Phase 5 (Fold back) because that phase requires the human's A/B answers. The point of the artifacts is to make the loop legible: every phase, every grade, every question is on disk and reviewable.
