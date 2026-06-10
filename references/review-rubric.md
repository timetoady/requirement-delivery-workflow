# Review Rubric

Use this for review-handling: scoring comments from any automated or peer reviewer
(for example Copilot, CodeRabbit, or human reviewers).

Score each substantive review item before deciding what to do.

## Score Bands

- `9-10`: high-signal correctness, security, regression, or maintainability issue
- `7-8`: strong improvement with clear upside and low downside
- `4-6`: reasonable but optional, contextual, or lower impact
- `1-3`: weak, redundant, incorrect, or not applicable

## Decision Labels

- `implement`
- `modify then implement`
- `decline with reason`

## Preferred Response Shape

- Accepted:
  - what was accepted
  - commit or change reference
  - what changed and why
- Accepted with modification:
  - what concern was valid
  - how the final fix differs from the suggestion
- Declined:
  - why it does not fit the repo, task scope, or current implementation

## Priorities

Prioritize findings that affect:
- stable public contracts
- environment/config correctness
- test coverage for new behavior
- runtime/drift safety
- misleading docs or non-portable references
