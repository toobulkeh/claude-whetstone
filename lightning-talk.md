# Lightning Talk: The Whetstone Loop

**Time:** 2–3 minutes
**Format:** Live terminal demo + 4 slides
**Setup:** Claude Code (or any agent CLI) projected, plus one tiny pre-picked task

---

## 1. Hook (15s)

> "I stopped building products. I started building factories."

I used to prompt my way to one-off wins. Now I sharpen skills that sharpen themselves.

This is the **Whetstone Loop**.

## 2. The Idea (30s)

Three moves:

1. **Distill** a conversation into a `skill.md`.
2. **Crank** the skill — run it, have the model grade itself, improve, repeat.
3. **Fold** your feedback back into the skill.

The skill is the asset. The chat is exhaust.

## 3. Live Demo (90s)

Tiny task: **"Write a one-line PR title from a diff."**

- **Turn 0 — cold.** Ask the model to do it with no guidance. Output is mediocre: vague, wrong tense, too long, includes the ticket prefix.
- **Turn 1 — distill.** *"Why was that bad? Write a `skill.md` so future-you doesn't repeat the mistakes."* Model writes rules: imperative mood, ≤50 chars, no ticket prefix, lead with the verb.
- **Turn 2 — crank.** Run the skill on the same diff. Better. Then: *"Grade your own output against your own rules. Fix it."* Model catches its own slip — it used "Updated" instead of "Update."
- **Turn 3 — feedback.** *"Ask me three A/B questions about edge cases you weren't sure about."*
  - "Refactors with no behavior change — `refactor:` prefix or plain verb? **A** / **B**"
  - "Multi-concern PRs — list both or pick the dominant? **A** / **B**"
  - I answer in 10 seconds. Model folds the answers into the skill.

By turn 3, the output is *meaningfully* better than turn 0 — and the skill is now a reusable artifact that gets sharper every time I use it.

## 4. Takeaway (20s)

This is the new way of working: explicit micromanagement on steroids.

Not micromanaging the model turn by turn. Micromanaging the *rules the model runs by* — captured in a file you can edit, share, version, and review.

You're not building the product. You're building the factory that builds the product.

---

## 5. Open Frontier (closer)

So far my more complex scenarios are **when to split vs. improve the loop** — how to handle conflicting reasoning with more detailed options — and **how to share and democratize** these compounded learnings, so my team can take advantage of them, not just me.

But ultimately I believe this is the new way of working — like a very explicit micromanager on steroids.

---

**Questions?**
