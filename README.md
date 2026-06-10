# requirement-delivery-workflow

A Claude Code / Agent **skill** that guides any development task through a strict, repeatable,
hard-gated delivery workflow — from requirement/context intake to closeout — with three things most
phased workflows lack:

- a **live objective tracker** that stays current every turn,
- an **independent critique and persona/UX review** (a fresh sub-agent that did not write the code,
  driving the running app as a real user when there is a UI), and
- a **bounded quality-gate scoring loop** that makes "good enough" an inspectable, evidence-backed bar.

It is tracker-agnostic: intake is whatever requirement/context material you provide (docs, pasted
text, links, design notes, or a ticket from any system), and closeout is wherever you want the result
summarized.

## Workflow

1. Intake and grounding
2. Requirements document creation
3. MVP delivery
4. Senior-dev pass
5. Independent critique and persona/UX review
6. Quality-gate loop
7. PR creation
8. Review handling (Copilot / CodeRabbit / human reviewers)
9. Closeout

Every phase ends with a hard stop and an explicit checkpoint; the workflow only advances on explicit
developer approval. Start it from **Plan Mode**.

## Install

This repo *is* the skill. Put it where your agent discovers skills, e.g.:

```bash
git clone https://github.com/timetoady/requirement-delivery-workflow \
  ~/.claude/skills/requirement-delivery-workflow
```

Then invoke it (for example `/requirement-delivery-workflow`) from within Plan Mode.

## Layout

```
SKILL.md                  # the skill definition + workflow
references/               # templates and methodology guides
  requirements-template.md
  requirements-example.md
  phase-handoff-template.md
  senior-dev-pass-checklist.md
  quality-rubric-template.md
  critique-persona-review.md
  live-objective-tracker.md
  pr-template.md
  review-rubric.md
  qa-smoke-template.md
  closeout-template.md
```

## Notes

- The "quality-gate" is a bounded heuristic self-evaluation rubric (score → iterate to a threshold or
  diminishing returns, capped, with user approval as the final gate), not a learned reward function.
- The persona/UX review uses whichever browser driver is available
  (Claude-in-Chrome / Claude Preview MCP → Playwright → Chrome DevTools → manual) and degrades to a
  developer-experience critique for non-UI work.

## License

MIT — see [LICENSE](LICENSE).
