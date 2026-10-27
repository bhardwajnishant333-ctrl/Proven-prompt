# Proven Prompt

Prompts that are tested before you use them.

Most prompt helpers rewrite your words and stop there, so nobody knows whether the new prompt actually works. Proven Prompt treats a prompt like code:

1. **Pins down the job**: the outcome you need, who reads the output, and where the prompt will run.
2. **Writes the checks first**: 4–7 pass/fail tests a stranger could apply, including at least one thing that must never happen (such as invented facts).
3. **Drafts the prompt**, fitted to where it runs: Claude chat, a project, Claude Code, an API system prompt, ChatGPT, Codex, or an image model.
4. **Tests it** on a typical input, a hard input and a trap input, in a fresh context that hasn't seen your conversation.
5. **Fixes the prompt, not the output**, and re-runs it until every check passes (up to three rounds).
6. **Delivers** the prompt ready to paste, what to fill in, where to run it, a pass/fail table and what changed. When you're improving a prompt, it also gives a before-and-after score.

Trivial prompts get a light mode: the prompt is written in a few lines and marked as not tested.

## When to use it

| You say | It does |
|---|---|
| "Write me a prompt I can reuse to turn meeting notes into action items" | Writes it, tests it on sample notes, and returns the prompt with its test table |
| "Improve this prompt: write a blog post about AI in healthcare" | Scores your original, improves it, re-tests it, and shows before and after |
| "Make a system prompt for my support bot" | Writes it with rules for refusals and handoffs, then tests it against tricky questions |
| "Quick prompt for a Slack status" | Writes it in a few lines with no test loop |

## Evidence it works

Each case was run with the skill and without it (plain Claude, same request). An independent agent graded the results against fixed checks in `skills/proven-prompt/evals/`.

| Test | With skill | Without skill |
|---|---|---|
| Reusable meeting-notes prompt | 5/5 | 1/5 |
| Improve a weak blog prompt | 5/5 | 1/5 |
| Trivial Slack-status prompt (should stay light) | 3/3 | 3/3 |

Without the skill, prompts were written but never run, so their quality was asserted rather than shown. With it, the blog prompt rose from 10/21 to 21/21 on its own checks. The original prompt invented statistics on the trap case; the improved one didn't.

To run the tests yourself, from the skill folder: `claude plugin eval . --allow-tools Write,Edit,Bash,Agent`

## Install

**Claude Code:**

```
/plugin install proven-prompt --marketplace bhardwajnishant333-ctrl/proven-prompt
```

**ChatGPT / Codex:** `skills/proven-prompt/` is a standard `SKILL.md` skill. For Codex, copy it to `~/.agents/skills/`. For ChatGPT, add it from the Skills panel in the desktop app.

## Data handling

The skill contains instructions only. It doesn't collect, store or send personal data. Test runs use whatever tools the host already provides.

## License

MIT. See [LICENSE](LICENSE).

Built by [Pivyon](https://pivyon.com).
