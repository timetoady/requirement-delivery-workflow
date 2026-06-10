# Quality Rubric Template

This defines the inspectable quality bar for the task and how the quality-gate loop uses it.
It is a heuristic self-evaluation rubric, not a learned reward function: the value comes from
making "good enough" explicit, scoring it with evidence, and iterating in a bounded way.

## Dimensions (score each 1-10)

- **Correctness** — does it do what the requirements say, including edge cases and error handling?
- **Scope-fit** — does it solve the stated objective without scope creep or missing must-haves?
- **Simplicity/DRY** — minimal surface area, no needless duplication, abstractions justified.
- **Test coverage** — the behavior that matters is proven by automated checks.
- **UX/DX** — for UI work, real user experience; for non-UI work, developer experience (API/CLI/docs ergonomics).
- **Docs clarity** — a reader can understand what changed and why.

Add or drop dimensions per task, but record the final set in the requirements doc.

## Score Bands

- `9-10`: excellent; no meaningful gap on this dimension
- `7-8`: solid; minor gaps acceptable to ship
- `4-6`: real gaps that should usually be closed before shipping
- `1-3`: not acceptable; must be addressed

## Passing Bar

- Default: every dimension >= 7 AND overall >= 8.
- Adjust per task (for example, raise correctness/test-coverage bars for high-risk changes).
- Record the chosen bar in the requirements doc so it is inspectable.

## Scoring Discipline (anti-reward-hacking)

- The scorer should be an agent that did **not** produce the work being scored.
- Tie scores to objective evidence wherever possible: tests pass, output matches spec, persona task completes.
- A score is not credible without a one-line justification and, where relevant, the evidence behind it.
- Do not let a score rise without a corresponding real change; "re-scored higher" with no diff is a red flag.
- Hard rule: a score may not increase unless a diff or a newly-passing check backs it. Reject any unbacked score bump.

## Quality-Gate Loop (bounded)

1. Score the current state against the rubric (independent scorer).
2. If below the passing bar AND under the iteration cap (default 2-3), dispatch improvement worker(s)
   with the specific named gaps to close.
3. Re-run relevant checks and re-score.
4. Stop when any of these is true:
   - the passing bar is reached
   - gains drop below ~1 point per iteration (diminishing returns)
   - the iteration cap is hit
   - if the cap is hit while still below the bar, stop and present options (ship-with-gaps / descope / escalate) rather than silently looping
5. Present a short score history: iteration -> per-dimension scores -> what changed.
6. The developer's approval is the final gate regardless of score. Record any accepted residual gaps.
