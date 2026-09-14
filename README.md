# Skills

Small, reusable agent skills for building software with clearer intent.

## Design before code

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

## Install

Install across projects and select your coding agent interactively:

```sh
npx skills@latest add Stixels/skills --skill design-before-code --global
```

For Codex specifically:

```sh
npx skills@latest add Stixels/skills --skill design-before-code --global --agent codex
```

Omit `--global` to install into the current project for sharing through Git.
Review and commit the installed files when using that approach.

## Use

Ask your agent:

> Use design-before-code to explore our team settings page. Show me disposable
> mockups and discuss the direction before implementing anything.

Or invoke `$design-before-code` in Codex.

For example, “make filtering easier” could become a persistent toolbar, an
on-demand filter panel, or filters integrated into the results. Compare the
interaction and use of space before polishing one of them.

To make it part of a project's workflow, add this to its tracked `AGENTS.md`:

> For meaningful UI or workflow changes, use the installed design-before-code
> skill. Ground decisions in this project's context, design system, and actual
> UI. Make layout, hierarchy, and interaction decisions visually reviewable, using
> disposable mockups or sufficient existing visuals, and agree on a direction
> before implementation. Choose mockup count and fidelity to resolve uncertainty.
> Carry forward existing approval; skip exploration for minor fixes and approved
> patterns. If the skill is missing, report that and follow this workflow directly.

This is guidance for the agent, not a technical enforcement mechanism.

## Update

```sh
npx skills@latest update design-before-code --global
```

Installation and update options are documented in the
[skills CLI](https://github.com/vercel-labs/skills).

## Skill source

- [design-before-code](skills/design-before-code/SKILL.md)

The repository structure and install experience are inspired by
[Matt Pocock's skills](https://github.com/mattpocock/skills).
The skill instructions are independently written.

## License

MIT — see [LICENSE](LICENSE).
