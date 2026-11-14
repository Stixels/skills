# Skills

mstack: the agent skills I use every day with Claude Code and Codex, usually
inside [T3 Code](https://github.com/pingdotgg/t3code). They cover reviewing and
shipping PRs, checking that a change works before calling it done, and how
long agent runs are planned and split across models. Most come from two packs,
edited to fit how I work:

- Theo Browne's [tstack](https://github.com/t3dotgg/tstack) (MIT, synced at
  `1085050`): review and shipping.
- Lauren Tan's [pstack](https://github.com/cursor/plugins/tree/main/harness/pstack)
  (MIT, synced at 0.15.36, `cursor/plugins@2776ddd`): the verification trust
  ladder, the playbooks and the principles.

`design-before-code` is my own: mockups before implementation (below).

## Install

Everything, for every coding agent you pick:

```sh
npx skills@latest add Stixels/skills --global --all
```

One skill:

```sh
npx skills@latest add Stixels/skills --skill council --global
```

Omit `--global` to install into the current project and share it through Git.
Update with `npx skills@latest update --global`.

Some skills lean on others, mostly on `mstack`'s shared `HARNESS.md` and
`MODELS.md`. Each one lists what it needs under `metadata.dependencies` and
says so at the top. Until the skills CLI installs those automatically
([vercel-labs/skills#2545](https://github.com/vercel-labs/skills/pull/2545)),
add them yourself:

```sh
npx skills@latest add Stixels/skills --skill tdd --skill mstack --global
```

Say `/mstack` in any thread for a short guide to which skill fits.

## Review and shipping (tstack)

| Skill | Say | What it does | Source |
|---|---|---|---|
| `council` | "council" | Opus and Sol review until both agree. Gemini comes along as the jester: heard, but no vote. | edited |
| `sol-loop` | "sol loop" | GPT Sol reviews a change in a loop until it approves. | as-is |
| `full-stop` | "full stop" | Proves, reviews and babysits a PR, then stops before merging. | as-is |
| `full-send` | "full send" | Same, and merges. Used by `full-stop`. | as-is |
| `file-pr` | "file a PR" | Files a short PR that leads with the problem. | as-is |
| `babysit-pr` | "babysit" | Watches a PR through CI and review bots. | as-is |
| `leaving-pr-comments` | (automatic) | Formats comments posted on my behalf. | as-is |

## Verification trust ladder (pstack)

Each rung replaces a judgment call with a check that runs.

| Skill | Say | What it does |
|---|---|---|
| `create-verification-skill` | `/create-verification-skill` | Writes a project skill that drives the real app the way a user does, with a feature map. |
| `maintain-verification-skill` | `/maintain-verification-skill` | Rechecks that skill and its feature map against the code and a live run. |
| `correct` | `/correct` | Turns a mistake agents keep making into a check that fails on it: architecture, then types, then a lint, then a test. |
| `tdd` | (automatic) or `/tdd` | A failing test first, when there is a cheap one to write. |
| `blast-radius` | `/blast-radius` | Finds what a change could break outside its diff, and proves the safe part by running code. |
| `interrogate` | `/interrogate` | Opus, Sol and Gemini try to break a change. Only findings proven by a run count. |
| `show-me-your-work` | `/show-me-your-work` | Keeps a decision log for long or unattended runs, then gets it checked by the other model family. |
| `benchmark-checklist` | (automatic) or `/benchmark-checklist` | Vets a performance number before anyone acts on it. |
| `reflect` | `/reflect` | At the end of a session, three reviewers find what went wrong and propose skill edits. Approved edits are committed to mstack, or opened as a draft PR for a repo skill. |

## How work runs (pstack)

| Skill | Say | What it does |
|---|---|---|
| `poteto-mode` | `/poteto-mode <task>` | Matches the task to a playbook (feature, bug fix, refactor, babysit, ...), copies its steps into a todo list, and applies the principles. |
| `architect` | `/architect` | Opus and Sol each sketch types and module shapes; the best parts are merged before code. |
| `arena` | `/arena` | Several attempts at the same task; pick a base and take the best parts of the rest. |
| `swarm` | `/swarm` | Many parallel workers, one report. |
| `figure-it-out` | `/figure-it-out` | Designs a playbook for a large or unusual task. |
| `how` / `why` | `/how`, `/why` | Explains how a system works, or why it was built this way. |
| `unslop`, `in-your-own-words` | `/unslop`, `/in-your-own-words` | Cleans up AI-sounding prose; restates the goal. |

The `principle-*` skills (laziness protocol, prove it works, fix root causes,
and 21 more) are the rules poteto-mode applies. Agents also pick them up on
their own when one fits, and you can call one by name.

## What changed from pstack

- **What runs on its own.** The `principle-*` skills, `tdd` and
  `benchmark-checklist` start no extra models, so agents may use them
  whenever they fit. Every other pstack skill runs only when named: `/name`
  in Claude Code, `$name` in Codex. Claude Code reads
  `disable-model-invocation: true`; Codex reads
  `policy.allow_implicit_invocation` in `agents/openai.yaml`. To switch a
  skill back, flip both.
- **Models.** pstack's Cursor defaults (Grok for code, Opus for judgment)
  are replaced by [`skills/mstack/MODELS.md`](skills/mstack/MODELS.md): Opus
  for code and judgment, Opus with Sol on every panel, and Gemini as the jester
  on `interrogate`. Sol and Gemini run through T3 Code's `delegate_task`.
- **Harness.** [`skills/mstack/HARNESS.md`](skills/mstack/HARNESS.md)
  replaces pstack's Claude Code adapter. It maps Cursor names to Claude Code,
  Codex and T3 Code. The skill text is otherwise unchanged, so syncing
  with pstack stays a clean diff.
- **`reflect`** writes approved edits into this repo (pull, commit, push)
  instead of a Cursor skills folder, and opens a draft PR for repo skills.
- **Left out:** skills unrelated to verification (`ketchup`, `make-bot-ui`,
  `statechart`, `technical-writing`, `bro`, and similar) and the
  formal-verification pair. With the pstack plugin installed, they still run
  as `/pstack:<name>`.

## Design before code (`design-before-code`)

Explore disposable UI mockups, clarify product intent, and agree on a user
experience before implementation.

Use it for new features and meaningful redesigns when the workflow, layout, or
information hierarchy is still undecided. Minor fixes and approved patterns
skip exploration.

The workflow:

1. Understand the user's job and the actual product behavior.
2. Explore approaches in proportion to the design uncertainty.
3. Make visual decisions reviewable with realistic disposable mockups or sufficient existing visuals.
4. Discuss the tradeoffs and agree on a direction.
5. Implement when requested, then inspect the real experience.

Mockups answer design questions. They are not production code by default.
There is no fixed number of options or mockups. Their number and fidelity depend
on what needs to be decided; layout and interaction decisions still require visual
review. Record consequential approved decisions in existing project documentation
when they need to outlive the conversation.
The skill uses your project's design system and works without a prescribed
frontend framework, Figma account, or custom MCP server. Your agent needs a way
to create and inspect visual artifacts for the full workflow.

Use it by asking your agent:

> Use design-before-code to explore our team settings page. Show me disposable
> mockups and discuss the direction before implementing anything.

## Working on these skills

To edit skills and have the changes apply right away, clone the repo and link
each skill instead of installing copies:

```bash
git clone https://github.com/Stixels/skills ~/Code/skills
for s in ~/Code/skills/skills/*/; do
  ln -sfn "$s" ~/.claude/skills/$(basename "$s")
  ln -sfn "$s" ~/.codex/skills/$(basename "$s")
done
```

`/reflect` commits approved skill edits to that clone.

## Requirements

- An authenticated [GitHub CLI](https://cli.github.com) (`gh`).
- T3 Code for the Sol and Gemini seats (`delegate_task`). Without it, those
  seats run on the parent model and the skill says so.
- `bun` for poteto-mode's TypeScript scripts.

## Syncing upstream

Diff each skill against its upstream before pulling changes in. Keep the
edits listed above. For pstack, re-run the model and adapter substitutions
described in "What changed from pstack".
