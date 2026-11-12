# mstack harness

The skills that came from pstack were written for Cursor. Read this file once
per session, then map each Cursor name with the tables below. If a Cursor
field has no row, drop it and say so. Never pass a field or value a tool's
schema rejects.

These skills run in Claude Code or Codex, usually inside T3 Code. "T3 tools"
means the T3 Code MCP tools such as `orchestrator_capabilities` and
`delegate_task`. They may be deferred: load them before the first call.

## Tools

| Cursor | Claude Code | Codex |
| --- | --- | --- |
| `Task` | `Agent` | `spawn_agent` |
| `AskQuestion` | `AskUserQuestion`. When it is absent, ask in plain text. | Ask in plain text. |
| Open a todolist | `TodoWrite` if offered. Otherwise keep the list in your reply and update it at each phase. | `update_plan` |
| Run another skill (`/name` or "the **name** skill") | Read `../<name>/SKILL.md` and follow it. | Same. |
| A skill that is not in mstack, such as `reflect`, `ketchup`, `no-comments` or `technical-writing` | Run `/pstack:<name>` when the pstack plugin is installed. Otherwise skip it with the line in "Cursor-only features". | Skip it with that line. |

## Subagent fields

| Cursor | Claude Code |
| --- | --- |
| `subagent_type: generalPurpose` | `subagent_type: "general-purpose"` |
| `poteto-agent` | `general-purpose`. Open the brief with: "You are a poteto-mode delegate, not its runner. Before any work, read `<absolute path to poteto-mode/SKILL.md>` in full, including its Principles section, and read each leaf `principle-*` skill you apply." |
| `comment-sicko` | `general-purpose`, briefed to remove each comment that doesn't explain a non-obvious why, and to change nothing else. |
| `readonly: true` | `subagent_type: "Explore"`. It keeps `Bash`, so the brief also says "do not edit files", and you check `git status` afterward. |
| `readonly: false` | `general-purpose` |
| `environment: "cloud"` | `isolation: "worktree"`. Name the branch or commit the worker checks out. For a Sol or Gemini worker, pass worktree isolation to `delegate_task`. |
| `environment: "local"` | Omit. |
| `cloud_base_branch: <b>` | Tell the worker to check out `<b>` first. |
| `run_in_background: true` | Omit. Subagents run in the background by default. |
| `resume` | Start a new agent and paste the earlier result into its brief. |
| `file_attachments` | List the paths in the brief. |

In Codex, `spawn_agent` covers the same fields. Use a separate worktree where
the table says `isolation: "worktree"`.

## Models

Role lines live in [MODELS.md](MODELS.md). Where a skill says "the role's
line", "the rule" or a default slug, read that file.

| Value | From Claude Code | From Codex |
| --- | --- | --- |
| `opus <effort>` | `Agent` with `model: "opus"` and `effort: "<effort>"` | T3 `delegate_task` to the latest Claude Opus, effort in `target.options` |
| `sol <effort>` | T3 `delegate_task` to the latest GPT Sol, effort in `target.options` | `spawn_agent` on the latest GPT Sol at that effort |
| `gemini` | T3 `delegate_task` to the latest Gemini Pro or Argon | Same |
| `inherit-parent`, `auto` | Omit `model` | Omit `model` |

Rules:

- Call `orchestrator_capabilities` once per thread before the first
  `delegate_task`, and pick models from it. Do not hardcode model ids.
- Give each `delegate_task` child the full brief. It sees none of this chat's
  history. Give a review child read-only instructions.
- If a model can't be started (no T3 tools, or the provider is missing), run
  that seat on the parent model and say so in the reply. A panel that ends up
  single-family must say so too. Never hide a substitution.
- Families: `opus` is Claude and `sol` is OpenAI. "Another family" or "a
  different model family" means the other one of these two.
- Gemini is the jester. Read what it says. Act on a Gemini finding only when
  a run proves it, when you confirm it in the code yourself, or when Opus or
  Sol agrees. Gemini never decides a vote, a base, or a verdict. If it can't
  be started, go ahead without it and say so.

## Paths

| Cursor | Here |
| --- | --- |
| `.cursor/skills/` | The skills folder the repo already uses (`.claude/skills/` or `.agents/skills/`). With none, use `.claude/skills/`. |
| `~/.cursor/skills/` | `~/.claude/skills/` and `~/.codex/skills/` |
| `.cursor/rules/*.mdc`, always-applied rules | `CLAUDE.md`, or `AGENTS.md` when the repo uses that. |
| `~/.cursor/plugins/.../pstack/<path>` or `https://github.com/cursor/plugins/blob/main/pstack/<path>` | The mstack copy at `../<path below skills/>` when it exists. Otherwise give the user the GitHub link to `cursor/plugins` under `harness/pstack/`. |
| Transcripts in `~/.cursor/projects/<slug>/agent-transcripts/` | Claude Code: `~/.claude/projects/<slug>/<session>.jsonl`. The slug is the workspace path with each `/`, `\`, `:` and `.` changed to `-`. Codex: `~/.codex/sessions/`. In T3 Code, read another thread with `t3_thread_read`. Read only this workspace's transcripts. |
| `.cursor/worktrees/...` | Wherever the harness put the worktree. Use `git worktree list`. |

## Cursor-only features

When a step needs one of these, put one line in your reply,
`Skipped here: <step>. <reason>.`, then go on. Do not stop, ask, or retry. If
a later step needs what the skipped step made, leave a placeholder and name it.

| Cursor | Here |
| --- | --- |
| Cursor Automations, routines, `update_state`, routine wakes, `SendToUser` | Skip. In T3 Code, `schedule_task` can replace a routine when the user asks for one. |
| Bugbot, "the agentic security review" | Any review bot on the PR (Codex review, Copilot, CodeRabbit, and so on). Triage its threads the same way. Skip a step that starts or configures Bugbot. |
| Cursor cloud agents | A worktree, as in Subagent fields. Skip a step that needs the cloud agent's own dashboard or machine. |
| Cursor's built-in babysit skill | mstack's `babysit-pr`. Inside a poteto-mode run, use its Babysit playbook instead. |
| `cursor-team-kit` skills: `deslop`, `control-ui`, `control-cli`, `create-skill`, `typescript-best-practices` | The repo's own equivalent when it has one. For `control-ui` and `control-cli`, that is its `verify-*` skill. For `create-skill`, follow poteto-mode's "Authoring a skill" playbook. Otherwise skip. |
| A built-in PR tool | `gh`, for example `gh pr create`. That is not a skip. |
| A Cursor restart | A session restart or context compaction. |

## Scripts

`poteto-mode/scripts/` uses `bun` for its TypeScript tools and `node` for
`check-plan.mjs`. If `bun` is missing, say so and skip that script.
