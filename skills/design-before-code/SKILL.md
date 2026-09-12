---
name: design-before-code
description: Clarify product intent through disposable UI mockups before implementing a new feature or redesign. Use when the user wants to explore UI approaches, compare layouts or workflows, or agree on a meaningful user-facing change before coding. Skip exploration for minor fixes and already-approved patterns.
---

# Design before code

Help the user decide how an experience should work before investing in production
implementation. Make the decisions visible through disposable mockups.

## Understand the job

Read the project's instructions, product context, design system, and relevant
feature behavior. Inspect the existing UI when available. Identify the user's
job, desired outcome, primary information and actions, and scope boundaries.

Separate facts from design choices: needing to filter results does not mean
needing a filter card. Answer questions from the codebase where possible. Ask
only about consequential unknowns, with a recommendation when you have one.
Existing UI is evidence, not an obligation to repeat a weak layout.

## Explore and show

Consider 2–3 materially different approaches to workflow, information hierarchy,
or interaction—not three visual treatments of the same arrangement. Consider
whether a better default or simpler behavior could eliminate new UI entirely.
Compare task completion, discoverability, space, and simultaneous decisions.

Create lightweight visual mockups of the strongest approach(es). Prefer the
project's actual theme and components with representative mock data. Use another
format when it answers the question more cheaply; no particular framework or
design tool is required. Render and inspect the mockups before presenting them.
Prose alone is not a visual mockup. If rendering is unavailable, explain that
limitation rather than claiming the design has been visually checked.

Keep mockups disposable and separate from production behavior. Build only enough
to answer the design question: no live writes, backend integration, or polishing
options that will be discarded. Do not overwrite unrelated work or promote
prototype code into production by default.

Use realistic content and data volumes. Include states that could change the
choice of design, such as empty versus crowded content, errors, or narrow screens.
Reuse the visual language while allowing different page compositions.

## Discuss the direction

Show the mockups, explain the important tradeoffs, and recommend one approach.
Ask for feedback on the concrete direction. Wait for design approval before
production implementation when the direction is unresolved. If pausing, explain
that this skill requires design approval and link to this instruction.

Carry forward approval already given. An explicit instruction to choose and
proceed also satisfies this step; silence does not. Do not reopen settled
choices or invent alternatives for minor corrections. A mockup-only request
ends with the proposal; approval of a design does not expand the task into an
implementation the user did not request.

## Implement and inspect

When implementation is requested and the direction is approved, build it using
the project's conventions. Identify affected surfaces before changing shared
components. Resolve routine details without another approval round; bring back
material departures from the agreed workflow for discussion.

Inspect the actual product in its surrounding screen, not just the prototype.
Exercise changed interactions, relevant states, and compact/wide layouts where
applicable. Check keyboard and focus behavior for changed controls.

Look for wasted space, excessive containers, weak hierarchy, competing actions,
unnecessary scrolling, controls separated from their content, and avoidable
choices. Balance density with readability. Keep essential actions discoverable.

Fix meaningful issues within the approved direction, then inspect again. Run
appropriate project checks and report what was actually verified. Source review
alone does not prove visual quality.
