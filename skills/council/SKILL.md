---
name: council
description: Trigger this if the user says 'council'. Gets review from Claude Opus and GPT Sol, with Gemini along as the jester, and loops until Opus and Sol agree.
---

# Council

Before an audit, address outstanding PR comments, fix known issues, and run
relevant checks. Give council the updated implementation; do not defer known
fixes until reviews return. If the user wants to discard the implementation
and explore new ideas, skip fixes to code being replaced.

## Members

- **Claude Opus** and **GPT Sol**: the voting members. Use the latest of each
  at high reasoning effort.
- **Gemini** (the latest Pro or Argon model): the jester. It reviews too, but
  it has no vote. Read what it says. Act on a Gemini finding only when you
  confirm it in the code yourself, or when Opus or Sol agrees once you put it
  to them. Do not chase Gemini's style notes or speculative concerns.

If the user names different members, use those. In T3 Code, use
`orchestrator_capabilities` to resolve the models, then `delegate_task` to
start one delegate per model in parallel. Elsewhere, use the harness's
subagent tools. If Opus or Sol is unavailable, report it instead of
substituting. If Gemini is unavailable, go ahead without it and say so.

Give each delegate the request and relevant context. For review or feedback,
keep their work read-only. Collect their responses and summarize the useful
findings, disagreements, and recommended next steps. Mark which findings came
only from Gemini and whether they held up.

## Rounds and scores

For a code review, ask each delegate for a score out of 10.
Count each full review pass as one round.
A fresh review after fixes starts a new round.
Later rounds can skip Gemini unless it raised something that held up.

If the work ends in a pull request, put the results in the PR description.
Put this section before the model and harness line at the end:

```markdown
## Council review

Rounds: 2

| Delegate | Round 1 | Round 2 |
| --- | --- | --- |
| Claude Opus | 7/10 | 9/10 |
| GPT Sol | 8/10 | 10/10 |
| Gemini (jester) | 6/10 | skipped |
```

If a delegate gives no score, write `no score`. Do not invent one.
If the PR is open, edit the description. Do not post a comment for this.
If there is no PR, put the same results in the final reply only.

## Finishing

If you agree with their recommendations, make the changes and request a fresh
review after. Continue until Opus and Sol are both satisfied. Gemini's
approval is not required.

Do not let repeated reviews result in scope creep. It is your job to fight to
keep scope limited.
