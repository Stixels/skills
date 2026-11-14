---
name: reflect
description: Spawn three parallel review subagents over the active transcript, surface learnings, and route each to a concrete edit on an existing skill. Use when the user says reflect.
disable-model-invocation: true
metadata:
  dependencies: "mstack poteto-mode"
---

> Needs `mstack`, `poteto-mode` installed next to this skill. If a `../<skill>/` file it points to is missing, tell the user to run `npx skills add Stixels/skills --skill mstack --skill poteto-mode`.

# Reflect

Read [the mstack harness](../mstack/HARNESS.md) once per session before you follow this skill. It maps the Cursor names in these pstack skills to Claude Code, Codex and T3 Code, and says how to run each model.

Mine the current conversation for durable learnings, then route them into skill edits.

Invoke when the user says "reflect" or "/reflect". Skip when the conversation is trivial, off-topic, or already covered by an existing skill the parent followed correctly. One-offs are not learnings.

## 1. Locate the active transcript

Find this conversation's transcript. Read only this workspace's transcripts. Globbing across every project reads private chats from unrelated work.

- **Claude Code:** `~/.claude/projects/<slug>/`, where the slug is the workspace path with each `/`, `\`, `:` and `.` changed to `-`. The newest `<session>.jsonl` directly in that folder is usually this session. Subagent transcripts sit in `<session>/subagents/`.
- **Codex:** the newest file under `~/.codex/sessions/` whose working directory is this workspace.
- **T3 Code:** if a file can't be found, read this thread with `t3_thread_read`.

Confirm the match: the first user message in the file must be this conversation's opening prompt. If nothing matches, pass a tight digest of the session instead.

## 2. Spawn three reviewers in parallel

One message, three subagents, `general-purpose`, not read-only. Reviewers need MCP access to look up the tickets, chat threads, and traces the transcript references.

Each reviewer and the synthesizer name a role line in [MODELS.md](../mstack/MODELS.md) and a default. Start each per HARNESS.md. Pick each subagent's reasoning level for the job, per the Subagents section of the **poteto-mode** skill.

| Lens | Role line | Default | Prompt template |
|---|---|---|---|
| Judgment | `reflect judgment, divergent, synthesizer` | `opus xhigh` | `references/judgment-reviewer.md` |
| Tooling | `reflect tooling` | `opus high` | `references/tooling-reviewer.md` |
| Divergent | `reflect judgment, divergent, synthesizer` | `opus xhigh` | `references/divergent-reviewer.md` |

Pass each template verbatim, substituting the transcript path or digest where marked. Reviewers return findings in their final reply.

## 3. Synthesize

One subagent, `general-purpose`, model from the `reflect judgment, divergent, synthesizer` line (default `opus xhigh`), not read-only. It spot-verifies citations through MCP. Pass `references/synthesizer.md` verbatim, with each reviewer's full output inlined where marked. It returns an Accepted / Rejected / Backlog list.

## 4. Structural enforcement check

Move any Accepted item that a lint rule, script, metadata flag, or runtime check would enforce more reliably to Backlog. See the **principle-encode-lessons-in-structure** skill. Backlog items in a repo are candidates for the **correct** skill.

## 5. Apply

Present the synthesizer's full Accepted / Rejected / Backlog output and wait for explicit approval before applying any Accepted edit. The user picks the subset and may redirect routings. Skill changes affect every future agent. Do not auto-apply.

Where each approved edit goes:

- **An mstack skill** (its folder links into `~/Code/skills/skills/`): edit it there. Run `git -C ~/Code/skills pull --ff-only` first. After the edits, commit with a message that names the learning, and push. The user's approval covers that commit and push. The Mac and Windows checkouts both pull from the same remote, so an unpushed edit is lost to the other machine.
- **A repo skill** (such as `.agents/skills/verify-app/`): make the edit on a new branch and open a draft PR with the **file-pr** skill. Never push to the default branch.
- **A pstack plugin skill** (`/pstack:<name>`, under `~/.claude/plugins/`): never edit the plugin's files. If the learning is worth keeping, copy that skill into `~/Code/skills/skills/`, apply the same harness and model edits the README lists, then edit the copy. Say that the skill moved into mstack.
- **Anything else**, such as a Claude or Codex built-in: put it in the Backlog with the reason.

Follow each approved row's Routing exactly:

- Trivial edit (a one-line bullet, a tightened sentence, a stale fact corrected): do it directly.
- Substantive edit (a new section, a new pattern table, more than about 10 lines), `tune description: <skill path>`, or `new skill via create-skill: <kebab-name>`: follow poteto-mode's "Authoring a skill" playbook (`../poteto-mode/playbooks/authoring-a-skill.md`).

Do not file Backlog items on your own. List them, and offer to file the repo ones as Linear issues in the matching team.

## 6. Summarize for the user

Short list, no preamble:

- Edits applied: `<skill path>`. What changed, one line each. Then the mstack commit, or the PR link.
- New skills created: `<skill path>`. One line each (rare).
- Backlog: `<title>`. One line each, and whether it was filed.
- Dropped: one line per rejected finding + reason from the synthesizer.
