# mstack models

One line per role. The pstack skills read these lines by role name. Edit a
value to change it, or delete a line to fall back to the skill's default.

Values:

- `opus <effort>`: Claude Opus, latest.
- `sol <effort>`: OpenAI GPT Sol, latest.
- `gemini`: Gemini, latest Pro or Argon. The jester: see HARNESS.md.
- `inherit-parent` or `auto`: run on the parent chat's model.

Effort ladder: `max` > `xhigh` > `high` > `medium` > `low`. Skills may move a
role one step up for the hardest work and down for routine work. They keep the
family.

[HARNESS.md](HARNESS.md) says how to start each model from Claude Code,
Codex and T3 Code.

```
# budget: high
feature, refactoring: opus high
bug-fix: opus high
perf-issue: opus high
hillclimb: opus high
judgment and prose: opus xhigh
hardest tasks: opus xhigh
how explorer: opus high
how explainer: opus xhigh
why investigators: opus high
why synthesizer: opus xhigh
reflect tooling: opus high
reflect judgment, divergent, synthesizer: opus xhigh
arena runners: opus xhigh, sol xhigh
arena cross-judge pool: sol xhigh, opus xhigh
swarm workers: opus high
architect runners: opus xhigh, sol xhigh
interrogate reviewers: opus xhigh, sol xhigh, gemini
```

Why these choices:

- Code delegates run on Opus as native subagents. That is the simplest path
  in Claude Code and needs no T3 call.
- Every panel pairs Opus with Sol, so "a different model family" always means
  a different provider.
- The cross-judge pool lists Sol first. The judge must be from a family other
  than the parent's, so a Claude chat gets Sol and a Codex chat gets Opus.
- Gemini only sits on the interrogate panel. There, a finding counts only
  once a run proves it, so its findings face the same check as the others.
