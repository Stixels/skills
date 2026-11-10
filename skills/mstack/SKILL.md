---
name: mstack
description: Explains what is in mstack and which skill to use. Use for /mstack, or when the user asks what mstack has, which skill fits a task, or how to change the models mstack uses.
disable-model-invocation: true
---

# mstack

mstack is the user's own skill pack. When the user asks which skill fits,
match the task to one or two skills below and say why. Do not run them unless
asked.

## Review and shipping (from tstack)

- `sol-loop`: GPT Sol reviews a change in a loop until it approves.
- `council`: Opus and Sol review until both agree. Gemini is the jester.
- `full-stop`: prove, review and babysit a PR to ready, never merge.
- `full-send`: the same, and merge.
- `file-pr`: file a short PR. `babysit-pr`: watch a PR through CI and bots.
- `leaving-pr-comments`: format for comments posted as the user.

## Verification trust ladder (from pstack)

- `create-verification-skill`: write a project skill that drives the real
  app like a user, with a feature map. Use when a repo has no scripted way
  to prove behavior.
- `maintain-verification-skill`: recheck that skill and its feature map
  against the code and a live run.
- `correct`: a mistake agents keep repeating becomes a check that fails on
  it (architecture, then types, then a lint, then a test).
- `tdd`: write a failing test first, when there is a cheap one.
- `blast-radius`: what a change could break outside its diff, proven by
  running code.
- `interrogate`: Opus, Sol and Gemini try to break a change. Only findings
  proven by a run count.
- `show-me-your-work`: a decision log for long or unattended runs, checked
  by the other model family.
- `benchmark-checklist`: vet a performance number before acting on it.
- `reflect`: at the end of a session, find what went wrong and propose skill
  edits. Approved edits go into mstack, or a draft PR for a repo skill.

## How work runs (from pstack)

- `poteto-mode <task>`: match the task to a playbook, copy its steps into a
  todo list, and apply the principles.
- `architect`: competing type and module sketches before code.
- `arena`: several attempts at one task, best base plus grafts.
- `swarm`: many parallel workers, one report.
- `figure-it-out`: design a playbook for a large or unusual task.
- `how`, `why`: how a system works, or why it was built this way.
- `unslop`, `in-your-own-words`: clean up prose, restate the goal.
- `principle-*`: the rules poteto-mode applies. Each can be called by name.

The `principle-*` skills, `tdd` and `benchmark-checklist` can start on their
own when they fit; they launch no extra models. Every other skill from pstack
runs only when the user names it: `/name` in Claude Code, `$name` in Codex.

## Shared files

- [HARNESS.md](HARNESS.md) maps Cursor names to Claude Code, Codex and T3
  Code, and says how to start each model.
- [MODELS.md](MODELS.md) sets the model for each role. To change a model,
  edit that file in the mstack repo and push.
