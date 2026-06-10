---
name: requirement-delivery-workflow
description: Guide any development task from requirement/context intake through planning, MVP delivery, independent critique and persona/UX review, a bounded quality-gate scoring loop, PR handling, automated/peer review response, and closeout. Use when work is organized around requirement or context documents (rather than a specific tracker) and the goal is a repeatable, gated delivery workflow with explicit handoffs, a live objective tracker, and an inspectable quality bar.
---

# Requirement Delivery Workflow

Use this skill when the work is organized around one or more requirement/context inputs and the goal is to move it through a strict, repeatable delivery workflow with explicit handoffs between phases, an independent critique step, and a measurable quality bar.

This is a tracker-agnostic generalization: the intake is whatever requirement and context material the developer provides, and the closeout is wherever the developer wants the result summarized. It adds three things beyond a basic phased workflow: a **live objective tracker**, an **independent critique and persona/UX review**, and a **bounded quality-gate scoring loop**.

Required inputs:
- one or more requirement/context inputs: requirement docs, pasted text, design notes, linked docs, URLs, or a ticket from any tracker
- any developer-supplied context from chat or linked material
- relevant local repo or repos

Non-negotiable rule:
- After planning is approved, create a requirements document before making any other code edits.
- The only repo-tracked edit allowed before the post-requirements checkpoint is creating/updating that requirements document itself.
- After creating the requirements document, stop and wait for explicit developer approval before making any code/config edits.
- Treat that document as the guiding star and middle-term memory for the rest of the task.
- Planning is not complete until the requirements document exists.
- Do not skip phases, compress phases, or auto-advance to the next phase without an explicit developer go-ahead.

Checkpoint protocol (hard requirement):
- Every phase ends with a hard stop and an explicit checkpoint prompt.
- Only a developer message sent after that checkpoint counts as approval to continue.
- Do not treat earlier broad instructions (for example "implement this plan") as permission to skip checkpoints.
- If a phase includes a developer-run validation step, stop and wait for their result before advancing.
- If there is any ambiguity, default to stopping and asking for explicit go-ahead.

Start rule:
- If the skill is invoked outside Plan Mode, stop and tell the developer to switch into Plan Mode first.
- Do not begin the delivery workflow until Plan Mode is active.

## Cross-Cutting Mechanisms

These apply across every phase, not just one.

### Live objective tracker
- Maintain an always-current view of work using the harness TODO tool (TodoWrite).
- The single `in_progress` item is the "current objective"; keep a short list of next actions and open items.
- Update it on every meaningful turn, not just at phase breaks.
- Boundary with the requirements document:
  - requirements document = the **durable** spec, decisions, and phase status; update it at phase boundaries.
  - live tracker = **volatile** in-session state; update it continuously.
- Do not duplicate long content into the tracker; keep it to objective, next action, and open items.
- See [references/live-objective-tracker.md](references/live-objective-tracker.md).

### Quality rubric and scoring
- Each task defines a quality rubric in its requirements document.
- Default dimensions, scored 1-10: correctness, scope-fit, simplicity/DRY, test coverage, UX/DX, docs clarity.
- Default passing bar: every dimension >= 7 and overall >= 8 (adjust per task and record the chosen bar).
- Scoring should be done by an agent that did **not** produce the work, to limit self-grading bias and reward-hacking.
- Prefer objective, checkable evidence (tests pass, output matches spec) over vibes.
- A rubric score may not increase without a backing diff or a newly-passing check; reject score bumps that are not tied to a real change.
- See [references/quality-rubric-template.md](references/quality-rubric-template.md).

### Supervisor / sub-agent pattern
- The main skill instance acts as the supervisor.
- It spawns sub-agents via the Agent tool for: (a) the independent critique/persona reviewer, and (b) improvement workers during the quality-gate loop.
- The supervisor scores sub-agent output against the rubric and either accepts it or returns it with specific, named gaps to close.
- Iteration is bounded (see the quality-gate phase). The developer's approval is always the final gate, regardless of score.

## Workflow

1. Intake and grounding
- Read all developer-supplied requirement/context inputs first (docs, pasted text, links, design notes, tickets).
- Inspect the local repo, docs, configs, and current implementation next, before asking questions.
- Initialize the live objective tracker with the working objective and first open items.
- Ask questions only when they materially change scope, success criteria, interfaces, or rollout.
- End the phase with:
  - a concise summary of what was learned
  - key decisions and open items
  - a direct prompt to approve planning and create the requirements document

2. Requirements document creation
- Produce a requirements doc before coding.
- Use [references/requirements-template.md](references/requirements-template.md).
- Create the file immediately after plan approval and before any other repo-tracked edits.
- Do not make code or config edits in the same turn as requirements doc creation unless a new developer message explicitly approves moving to MVP after reviewing the doc.
- Default path rules:
  - use an existing repo working-docs location if one clearly exists
  - otherwise create `docs/working/<slug>-requirements.md`, where `<slug>` is a short kebab-case name derived from the objective
  - if a repo should not gain temporary docs, create `.codex/working/<slug>-requirements.md`
- Include checkable TODOs, acceptance criteria, test/QA expectations, and the task's quality rubric (dimensions + chosen passing bar).
- For multi-repo work, use one master requirements document and track repo-specific work inside it.
- Keep the requirements doc updated through MVP, senior-dev review, critique, PRs, review handling, and closeout.
- End the phase with:
  - the requirements document path
  - a summary of the captured objective, scope, TODOs, and quality bar
  - a direct prompt to begin MVP development
- Hard stop after this phase:
  - Do not run baseline local verification unless the developer explicitly asks for it now.
  - Do not make code edits or config edits until the developer explicitly confirms to proceed.
  - Treat prior "implement this" instructions as superseded until this checkpoint is explicitly cleared.

3. MVP delivery
- Do not start code edits until the requirements doc is created.
- If the approved plan requires baseline behavior verification before changes, run that baseline step first and stop for developer confirmation when they need to execute or validate it locally.
- Implement the smallest useful version that satisfies the objective.
- Prefer minimal code, clear naming, and limited surface-area changes.
- Require tests or other verifiable checks before calling MVP complete.
- Do not skip required repo conventions just to move faster.
- Update the requirements document and the live tracker with implementation status, test results, and remaining TODOs.
- End the phase with:
  - what was implemented
  - what tests or checks passed
  - remaining gaps or known limitations
  - a direct prompt to begin the senior-dev pass

4. Senior-dev pass
- Run a distinct cleanup/review pass after MVP and passing checks.
- Use [references/senior-dev-pass-checklist.md](references/senior-dev-pass-checklist.md).
- Run this phase in two explicit sub-passes:
  - Regression/risk hardening pass:
    - prioritize behavioral correctness, edge cases, error handling, and regression prevention in the highest-risk code paths
    - add/adjust tests when needed to prove risk reductions
  - DRY/minimal-code sweep:
    - remove duplication and unnecessary branching/state/overrides
    - tighten naming and structure so the implementation is as small and clear as possible without changing scope
- Review for DRYness, minimal code, maintainability, standards alignment, and comment quality.
- Tighten code and docs only where it improves clarity or safety.
- Update the requirements document with cleanup work, resolved concerns, and any follow-up notes.
- End the phase with:
  - what was improved and why (separately summarize risk-hardening changes and DRY/minimal-code reductions)
  - any residual risks
  - a direct prompt to begin the independent critique and persona review

5. Independent critique and persona/UX review
- Spawn a fresh reviewer sub-agent that did **not** write the code, so the critique is a genuine second opinion.
- Give the reviewer the objective, the requirements doc, the rubric, and the change surface; do not give it the author's rationalizations.
- Have the reviewer evaluate against the rubric and produce a scored critique with specific, named gaps.
- For projects with a user interface:
  - launch the running app and drive it as a target user persona, capturing screenshots and observed behavior
  - use whichever browser driver is available, in this fallback order: Claude-in-Chrome or Claude Preview MCP -> Playwright -> Chrome DevTools (CDP / chrome-devtools MCP) -> manual screenshot-and-describe
  - if no driver is available and the developer cannot supply screenshots or a recording, do not guess a UX number: inspect the UI from code/markup, score UX/DX from that, and mark UX/DX as `unverified` rather than inventing a score
  - critique real UX: clarity, flow, error states, accessibility basics, and whether a real user can complete the core task
- For non-UI work (libraries, CLIs, APIs):
  - critique developer experience instead: API ergonomics, CLI output and help, error messages, naming, and docs
- See [references/critique-persona-review.md](references/critique-persona-review.md).
- Update the requirements document with the critique summary and the initial rubric scores.
- End the phase with:
  - the persona/critique findings and per-dimension scores
  - the gaps that fall below the passing bar
  - a direct prompt to begin the quality-gate loop (or to accept as-is if already above the bar)

6. Quality-gate loop
- Goal: close the gaps from the critique until the work clears the passing bar, without runaway cost.
- Loop (bounded):
  - the supervisor scores the current state against the rubric
  - if it is below the passing bar and under the iteration cap (default 2-3 iterations), dispatch improvement worker(s) with the specific named gaps to close
  - re-run relevant checks and re-score
  - stop when any of these is true: the passing bar is reached, gains drop below ~1 point per iteration (diminishing returns), or the iteration cap is hit
  - if the cap is hit while still below the passing bar, stop and present options (ship-with-gaps / descope / escalate); never silently keep looping
- Guard against reward-hacking:
  - keep scoring tied to objective evidence where possible
  - the scorer should not be the same agent that just made the improvement
- Present a short score history (iteration -> scores -> what changed) and any remaining gaps.
- Update the requirements document with final scores and any accepted residual gaps.
- Hard stop: the developer's approval is the final gate regardless of score.
- End the phase with:
  - the score history and final scores
  - remaining gaps and your recommendation (ship / keep iterating / descope)
  - a direct prompt to begin PR creation

7. PR creation
- Draft PR descriptions using [references/pr-template.md](references/pr-template.md).
- If the repo uses automated reviewers (for example Copilot or CodeRabbit) or required human reviewers, verify reviewer automation and add reviewers if needed. This step is optional and conditional on the repo's setup.
- Update the requirements document with PR links, repo coverage, and review-readiness status.
- End the phase with:
  - PR links
  - what each PR covers
  - whether reviewer automation and reviewers are in place
  - a direct prompt telling the developer to wait for or trigger review before continuing

8. Review handling
- Applies to any automated or peer reviewer (for example Copilot, CodeRabbit, or human reviewers).
- Do not start this phase until the developer confirms review comments are populated on the PRs.
- Retrieve and process review comments one at a time.
- Score findings using [references/review-rubric.md](references/review-rubric.md).
- Explain whether each comment is accepted, modified then implemented, or declined with reason.
- Hard stop before implementation:
  - first present the retrieved comments, rubric-based scoring, and your recommended disposition for each comment
  - do not make any code, test, doc, or runtime-sync changes yet
  - wait for an explicit developer approval message before implementing any accepted or modified comments
  - if the developer wants only a subset implemented, treat that as the new scope for this phase
- After approval, implement only the approved follow-up changes.
- Rerun relevant checks after code changes.
- Update the requirements document with review status and follow-up work.
- End the phase with:
  - review outcome summary
  - follow-up changes made
  - current PR status
  - a direct prompt to begin closeout

9. Closeout
- Produce a concise delivery summary using [references/closeout-template.md](references/closeout-template.md).
- Write the summary into the requirements document, and post it wherever the developer wants it: a PR comment, an issue/tracker comment, a chat update, or a status doc.
- Include PR links, validation summary, and QA guidance.
- Update the requirements document with final status, QA references, and closeout notes.
- End the phase with:
  - the closeout summary
  - final PR state
  - links to QA guidance and the requirements doc

## Operating Rules

- Treat the workflow as hard-gated.
- Always wait for explicit developer feedback before moving from one phase to the next.
- Do not combine phases for speed.
- Express lane (exception): for genuinely trivial changes, the developer may explicitly pre-approve running the build-and-harden phases (MVP through the quality-gate loop) in a single pass. Absent that explicit pre-approval, keep every gate.
- Do not treat "keep going" implications as approval if the phase checkpoint has not been surfaced explicitly.
- Treat approvals as phase-local and non-transferable; each checkpoint requires fresh confirmation.
- Keep the next phase in mind and remind the developer what comes next, but stop until they confirm.
- The same sequence applies even when the work spans multiple repos or PRs.
- Keep the live objective tracker current throughout; keep the requirements document current at phase breaks.

## Output Expectations

For requirement-delivery requests, help the developer produce:
- a grounded summary of the requirements and current state
- a requirements doc file with checkable TODOs, a quality rubric, and live phase tracking
- a tested MVP
- a senior-dev review summary with concrete improvements
- an independent critique with persona/UX findings and rubric scores
- a quality-gate score history showing the work clearing (or consciously not clearing) the bar
- a PR body in the agreed format
- review responses with acceptance rationale
- a closeout summary with PR links and QA notes
- a clear end-of-phase checkpoint after every phase

## References

- Requirements template: [references/requirements-template.md](references/requirements-template.md)
- Requirements example: [references/requirements-example.md](references/requirements-example.md)
- Phase handoff template: [references/phase-handoff-template.md](references/phase-handoff-template.md)
- Senior-dev checklist: [references/senior-dev-pass-checklist.md](references/senior-dev-pass-checklist.md)
- Quality rubric template: [references/quality-rubric-template.md](references/quality-rubric-template.md)
- Critique and persona review: [references/critique-persona-review.md](references/critique-persona-review.md)
- Live objective tracker: [references/live-objective-tracker.md](references/live-objective-tracker.md)
- PR template: [references/pr-template.md](references/pr-template.md)
- Review rubric: [references/review-rubric.md](references/review-rubric.md)
- QA smoke template: [references/qa-smoke-template.md](references/qa-smoke-template.md)
- Closeout template: [references/closeout-template.md](references/closeout-template.md)
