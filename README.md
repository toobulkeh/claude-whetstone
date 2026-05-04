# claude-whetstone

> Skills that sharpen themselves.

A technique for working with coding agents (Claude Code, Cursor, etc.) where you stop optimizing one-off prompts and start sharpening a durable artifact: a `skill.md` that gets better every time you use it.

You're not building the product. You're building the factory.

## The loop, in one breath

1. Talk through the problem with the model.
2. Have the model distill the conversation into a `skill.md`.
3. Run the skill on a real task. Have the model grade its own output and improve. Repeat.
4. Have the model ask *you* A/B questions about the edge cases it hit.
5. Fold your answers back into the skill.
6. Future runs go through the skill. New learnings update the skill. The chat is exhaust.

## Read these in order

- [`skill.md`](./skill.md) — the skill itself, written for an agent to execute. Drop it at `~/.claude/skills/whetstone/SKILL.md` to install.
- [`examples/`](./examples/) — two captured runs: Whetstone on a `pr-title` skill, and Whetstone on its own `skill.md` (the meta-loop).
- [`lightning-talk.md`](./lightning-talk.md) — a 2–3 minute talk with a live demo, if you want to share it with your team.

## Install

```sh
mkdir -p ~/.claude/skills/whetstone
cp skill.md ~/.claude/skills/whetstone/SKILL.md
```

Then in Claude Code: `run the whetstone loop on <path-to-target-skill>` with a real test input.

## Open frontier

- **Split vs. improve.** When two reasoning paths conflict inside one skill, fork or thread?
- **Democratization.** A skill is only valuable if the team uses it. How do these compound across people?

## License

MIT. Use it, fork it, improve it. PRs welcome — especially ones that update the skill itself.
