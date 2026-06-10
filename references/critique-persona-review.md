# Critique and Persona/UX Review

This phase produces a genuine second opinion. The reviewer is a fresh sub-agent that did **not**
write the code, evaluating the work against the quality rubric.

## How to run it

1. Spawn a reviewer sub-agent via the Agent tool.
2. Give it: the objective, the requirements doc, the rubric, and the change surface (files/PR/app URL).
   Do **not** feed it the author's rationalizations — let it judge the artifact, not the defense.
3. Ask it to score each rubric dimension 1-10 with a one-line justification and name specific gaps.
4. The supervisor folds the critique and scores into the requirements doc.

## UI projects: drive the running app as a persona

Pick a target user persona (e.g. "first-time user", "power user in a hurry", "user on a slow connection")
and actually complete the core task as that persona, capturing screenshots and observed behavior.

Use whichever browser driver is available, in this fallback order:

1. **Claude-in-Chrome MCP** — navigate, read_page, computer/click, screenshot.
2. **Claude Preview MCP** — preview_start, preview_screenshot, preview_click, preview_fill, preview_inspect.
3. **Playwright** — `npx playwright` scripts or a Playwright MCP, if installed.
4. **Chrome DevTools** — CDP via a `chrome-devtools` MCP or a DevTools-protocol client.
5. **Manual fallback** — ask the developer for screenshots or a screen recording and critique from those.

Detect what is available first; if none of 1-4 are present, degrade to the manual fallback rather than
blocking. Note in the critique which driver was used (or that it was manual).

If there is no visual access at all (no driver, and the developer cannot supply screenshots or a
recording), do not invent a UX score: inspect the UI from code/markup, base UX/DX on that, and mark
UX/DX as `unverified` in the scores rather than guessing a number.

Critique real UX:
- can the persona complete the core task without confusion?
- clarity of labels, flow, and primary calls to action
- error and empty states
- accessibility basics (focus order, contrast, keyboard reachability, alt text)
- responsiveness/layout at a couple of viewport sizes

## Non-UI work: critique developer experience instead

For libraries, CLIs, and APIs there is no screen to drive, so evaluate DX:
- API ergonomics: are the common calls obvious, hard to misuse, and well-typed?
- CLI: is `--help` clear, are errors actionable, is output readable/pipeable?
- error messages: do they say what went wrong and what to do?
- naming and docs: would a new caller get it right on the first try from the README/examples?

## Output

- per-dimension rubric scores with one-line justifications
- the persona used and the driver used (or manual)
- specific, named gaps that fall below the passing bar
- screenshots/observations for UI work, or concrete DX examples for non-UI work
