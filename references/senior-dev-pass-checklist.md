# Senior Dev Pass Checklist

Run this after the MVP works and checks are passing.

## Code Quality

- Is any logic duplicated and worth consolidating?
- Is the solution smaller than it started, or at least no bigger than necessary?
- Are names clear and consistent?
- Are abstractions justified, or are they hiding simple code?

## Maintainability

- Does the code follow repo conventions and surrounding patterns?
- Are comments sparse but useful?
- Are there fragile assumptions that should be documented or removed?
- Did docs/config/examples drift from the implementation?

## PR Readiness

- Would another senior engineer understand why the change exists?
- Are tests proving the behavior that actually matters?
- Are edge-case or regression risks called out clearly?
- Is there any accidental repo- or environment-specific detail that should be generalized?
