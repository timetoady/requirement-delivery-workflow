# Requirements Template

Use this for the planning or requirements phase when the task is non-trivial.

Creation rule:
- Create this file immediately after the plan is approved.
- Do this before any other code edits.
- Keep it updated through MVP, senior-dev pass, critique, PR creation, review handling, and closeout.

Best-practice notes:
- Keep it concise and useful; avoid bloated specs that people will not read.
- Treat it as the one working source of truth while implementation is active.
- Include assumptions, out-of-scope items, and success criteria explicitly.
- Prefer linking out to deep detail instead of stuffing every note into the document.
- Make TODOs checkable and reviewable, not vague reminders.
- For multi-repo work, keep one master requirements document and track repo-specific work inside it.
- Update the document at every phase break so it stays aligned with the real state of the work.

Default file-location preference:
1. existing repo working-docs area if one clearly exists
2. `docs/working/<slug>-requirements.md`
3. `.codex/working/<slug>-requirements.md` when the doc should stay temporary

(`<slug>` is a short kebab-case name derived from the objective.)

## Phase Status

- Intake and grounding:
- Requirements document creation:
- MVP delivery:
- Senior-dev pass:
- Independent critique and persona review:
- Quality-gate loop:
- PR creation:
- Review handling:
- Closeout:

## Summary

- Requirement source(s):
- Requirement doc path:
- Objective:
- User/developer problem:
- Success criteria:

## Current State

- What exists already:
- Relevant repos/workspaces:
- Known constraints:
- Supplemental context used:

## Scope and Defaults

- In scope:
- Out of scope:
- Assumptions/defaults chosen:

## Implementation Plan

- Approach:
- Public interface or behavior changes:
- Important internal changes:
- Risks or edge cases:

## Quality Rubric

Dimensions scored 1-10 (see quality-rubric-template.md). Record the chosen passing bar.

- Correctness:
- Scope-fit:
- Simplicity/DRY:
- Test coverage:
- UX/DX:
- Docs clarity:
- Passing bar (default: every dimension >= 7 and overall >= 8):

## Checkable TODOs

- [ ] Create the requirements document file
- [ ] Confirm current behavior and linked context
- [ ] Implement MVP
- [ ] Add or update tests/checks
- [ ] Run senior-dev pass
- [ ] Run independent critique and persona/UX review
- [ ] Clear the quality bar via the quality-gate loop
- [ ] Create PRs
- [ ] Handle review comments
- [ ] Post closeout summary with PRs and QA notes

## PR and Review Tracking

- Repo and branch coverage:
- PR links:
- Reviewer automation status:
- Review status:
- Closeout status:

## Test and QA

- Automated checks:
- Manual QA:
- Smoke-test prompts or scenarios:

## Quality-Gate Score History

- Iteration -> per-dimension scores -> what changed:
- Final scores:
- Accepted residual gaps:

For a concrete example, see [requirements-example.md](requirements-example.md).
