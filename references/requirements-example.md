# Example Requirements Document

Use this as a compact example of the pattern, not as a rigid spec.

---

# Product Guide Knowledge Asset

## Summary

- Requirement source(s): `product-guide-requirements.md` (shared design doc) + linked architecture notes
- Requirement doc path: `docs/working/product-guide-knowledge-asset-requirements.md`
- Objective: Add a curated product-guide knowledge asset and inject it into assistant prompt context so users can get reliable navigation and orientation answers.
- User/developer problem: The assistant falls back to generic answers because it lacks grounded product-specific guidance about where things live and what major areas do.
- Success criteria:
  - supported navigation questions are answered with the correct section or page type first
  - unsupported questions decline safely without inventing product details
  - prompt assets are shared across local and deployed runtime paths

## Current State

- What exists already:
  - the assistant integration is live in at least one lane
  - prompt behavior exists, but product-guide knowledge is incomplete or not consistently deployed
- Relevant repos/workspaces:
  - assistant backend repo
  - API or facade repo
  - web or client repo
- Known constraints:
  - keep public API contracts stable
  - avoid lane-specific assumptions in reusable prompt assets
  - support both local and deployed runtime paths
- Supplemental context used:
  - current public product/site structure
  - internal design or architecture docs
  - local repo implementation shape

## Scope and Defaults

- In scope:
  - curated guide content
  - prompt injection
  - generated prompt assets
  - tests and QA prompts
  - PR and closeout
- Out of scope:
  - full retrieval or search architecture
  - broad domain question-answering outside the guide
  - unrelated hardening work not required by this task
- Assumptions/defaults chosen:
  - use one canonical guide file
  - use one canonical prompt policy source
  - keep the guide concise and navigation-oriented

## Implementation Plan

- Approach:
  - author canonical prompt assets
  - generate shared prompt bundles for local and runtime consumption
  - inject prompt policy plus guide content into the system prompt
  - validate with targeted smoke questions
- Public interface or behavior changes:
  - no public endpoint changes
  - improved assistant answer quality for navigation and orientation prompts
- Important internal changes:
  - startup prompt asset loading
  - drift checks
  - prompt inspection tooling
  - runtime packaging alignment
- Risks or edge cases:
  - runtime packaging drift
  - generic answers if assets are stale or not deployed
  - prompt bloat if guide content grows too much

## Quality Rubric

- Correctness: 9
- Scope-fit: 9
- Simplicity/DRY: 8
- Test coverage: 8
- UX/DX: 8 (persona: a confused user asking "where do I find X?")
- Docs clarity: 8
- Passing bar: every dimension >= 7 and overall >= 8

## Checkable TODOs

- [x] Create the requirements document file
- [x] Confirm current behavior and linked context
- [x] Implement MVP
- [x] Add or update tests/checks
- [x] Run senior-dev pass
- [x] Run independent critique and persona/UX review
- [x] Clear the quality bar via the quality-gate loop
- [x] Prepare PR description
- [x] Handle review comments
- [x] Post closeout summary with PRs and QA notes

## Test and QA

- Automated checks:
  - prompt asset tests
  - runtime prompt asset tests
  - API or facade tests
- Manual QA:
  - assistant entry point is visible and available in the expected client
  - health endpoints return success in the target lane
  - smoke prompts produce section-first answers
- Smoke-test prompts or scenarios:
  - `Where can I find videos on this site?`
  - `What is the calendar or timeline page for?`
  - `Where do I find person or profile pages?`
  - `Where can I browse places, locations, or maps?`
  - `What should I use if I do not know which section contains something?`

## Quality-Gate Score History

- Iteration 1 -> UX/DX 6 (persona could not tell when an answer was a decline) -> added explicit decline phrasing and a smoke prompt for it
- Iteration 2 -> UX/DX 8, all dimensions >= 7, overall 8 -> passed
- Final scores: see Quality Rubric above
- Accepted residual gaps: guide content breadth is intentionally narrow for MVP

---

Why this is a good pattern:
- concise enough to stay readable
- specific enough to guide implementation
- tracks scope, assumptions, success criteria, and an inspectable quality bar
- gives the team a living checklist during delivery
