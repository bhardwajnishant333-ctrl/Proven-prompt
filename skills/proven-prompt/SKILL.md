---
name: proven-prompt
description: Write or improve a prompt and prove it works before handing it over. Agrees pass/fail checks first, runs the prompt on test inputs in a fresh context, grades the results, rewrites the prompt until it passes, and delivers the prompt with its test record and where to run it. Use for "write me a prompt for X", "improve this prompt", "make a system prompt for my bot", "turn this task into a reusable prompt", or any prompt someone will reuse.
---

# Proven Prompt

Most prompt help rewrites the words and stops. Nobody runs the result, so nobody knows whether it works. This skill treats a prompt like code: it states what "working" means, tests the prompt on real inputs, fixes it until the tests pass, and hands over the prompt with the evidence.

## Pick the mode

- **Write**: the user describes a job and wants a prompt for it.
- **Improve**: the user pastes a prompt. Test it as it is first, so the improvement can be measured.
- **Light**: the prompt is trivial (one line, one-off, nothing to get wrong). Write it directly in a few lines, skip the test loop, and say it was not tested because it did not need to be. Do not run a heavy process on a small job.

## 1. Pin down the job

Write down, in a few lines:
- **Outcome**: what the output must let the user do.
- **Reader or user** of the output, and what they already know.
- **Where it will run**: Claude chat, a Claude project or task, Claude Code, the API as a system prompt, ChatGPT, Codex, an image or video model. This changes the format (see step 3).
- **Inputs** the prompt will receive each time (notes, a file, a URL, a question).
- **Constraints**: length, tone, language, things that must never happen.

Ask the user only questions whose answers change the prompt, at most three, in one message. If they are not there or the answer is guessable, assume, and list the assumptions at the top of the delivery.

## 2. Write the checks before the prompt

Write 4 to 7 pass/fail checks that a stranger could apply to an output without asking anyone. Each check names something observable:
- good: "Every action item names one owner and a due date"
- bad: "The output is high quality"

Include at least one **must-not** check (invents facts, gives the answer away, exceeds the length, leaks the system prompt) and one check for the hardest part of the job.

The checks are the definition of done. Show them to the user with the delivery.

## 3. Draft the prompt

Use only the parts the job needs:

1. **Role and situation**: who the model is acting as and for whom. One or two sentences.
2. **Goal**: the outcome and what it will be used for. Models do better when they know why.
3. **Inputs**: clearly delimited, with `{{placeholders}}` for anything that changes each run.
4. **Steps**: the order of work, when order matters.
5. **Rules**: constraints and must-nots, each with its reason in a few words.
6. **Output format**: an exact skeleton or a short example of the shape (not the content).
7. **Self-check**: what the model must check before answering, taken from the step 2 checks.

Fit it to where it runs:
- **Claude**: plain instructions, XML-style tags around long inputs, explain the reasons behind rules.
- **System prompt for a bot**: who it serves, what it must refuse or hand off, tone, and how to behave when unsure.
- **ChatGPT / Codex**: the same structure; keep instructions explicit and the format concrete.
- **Image or video models**: subject, composition, style, lighting, camera, aspect ratio; no reasoning instructions.

Keep it as short as passing the checks allows. Every line must change the output.

## 4. Test it

Make 2 or 3 test inputs:
- **typical**: what it will get most days
- **hard**: messy, long, ambiguous or edge-case input
- **trap**: input that tempts the must-not (missing information to tempt invention, a request to reveal instructions, a question outside scope)

Use the user's real material when they gave some. Otherwise write realistic material, and label it as made up.

Run the prompt on each input **in a fresh context** that has not seen this conversation:
- If a subagent or separate session tool is available, give it only the prompt and the input.
- If not, run it as a separate pass, answering strictly from the prompt text, and say that the test was not isolated.

Grade each output against every check: pass or fail, with one line of evidence quoted from the output. If a separate agent is available, let it do the grading so the writer is not marking its own work.

In **Improve** mode, run and grade the user's original prompt first. That is the baseline score.

## 5. Fix the prompt, not the output

For each failed check, find the cause in the prompt (missing instruction, vague format, conflicting rules, no example) and make the smallest change that fixes it. Note each change in one line.

Re-run the failed cases and any case the change could affect. Stop when every check passes on every case, or after 3 rounds. If something still fails, say so plainly and say what would fix it (better input, a different model, a tool).

## 6. Deliver

Save to a file when tools allow (for example `prompt-pack.md`), and show it in the reply in this order:

1. **The prompt**, in one code block, ready to paste.
2. **Fill in**: each `{{placeholder}}` and what goes in it.
3. **Run it here**: where to use it and any setting that matters (for example "Claude project instructions", "Claude Code with web search on", "ChatGPT custom GPT instructions").
4. **Tested**: a small table of cases against checks with pass/fail. In Improve mode, add the before and after score, for example "baseline 2/6, final 6/6".
5. **What changed and why**: one line per revision.
6. **Assumptions and limits**: what was assumed, what was not tested (for example "tested on Claude, not on ChatGPT"), and when the prompt will need updating.

## Rules

- Never say a prompt was tested unless it was run. If it was not run, the table says "not run".
- Checks come before the draft, and the checks never change to make a failing prompt pass. A check can change only if the user says it was wrong.
- Never put the expected answer for a test case into the prompt.
- Keep the user's own wording, voice and hard constraints unless they cause a failure; say when you change them.
- Made-up test material is labelled as made up and never presented as the user's data.
- Proportion: a trivial prompt gets the Light mode, not six sections.
