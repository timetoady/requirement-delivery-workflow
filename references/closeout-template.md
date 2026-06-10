# Closeout Template

Use this after PRs and review state are in good shape. Write the summary into the requirements
document, and post it wherever the developer wants it: a PR comment, an issue/tracker comment, a
chat update, or a status doc.

## Delivery Summary

PRs ready for review / merged:

- PR link
  - one-line summary
- Additional PR link
  - one-line summary

Validation completed:

- behavior/config/output verification summary
- quality-gate final scores (and any accepted residual gaps)
- smoke-test or QA doc reference
- review feedback handled

## QA Comment

Cut-down QA smoke test:

1. Confirm the feature appears and opens/loads normally.
2. Run the core prompts or scenarios.
3. Check expected behavior:
   - correct primary behavior
   - concise directional guidance where relevant
   - no obvious fabrication/regression
4. Check no visible UI/API regressions.

If deeper validation is needed, reference the fuller QA doc or scenario list.
