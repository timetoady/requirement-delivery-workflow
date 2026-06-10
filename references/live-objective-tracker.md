# Live Objective Tracker

A lightweight, always-current view of the work that complements (does not replace) the requirements doc.

## What it is

Use the harness TODO tool (TodoWrite) as the live tracker:
- exactly one item is `in_progress` at a time — that item is the **current objective**
- the rest are short, concrete next actions and open items
- check items off and add new ones as work proceeds

## Update cadence

- Update the tracker on **every meaningful turn**, not just at phase breaks.
- When you start a unit of work, mark it `in_progress`; when done, mark it `completed`.
- When a phase checkpoint is approved, reflect the new phase's first actions in the tracker.

## Boundary with the requirements document

| | Live tracker (TodoWrite) | Requirements document |
|---|---|---|
| Nature | volatile, in-session | durable, source of truth |
| Update frequency | every meaningful turn | at phase boundaries |
| Holds | current objective, next actions, open items | spec, decisions, scope, rubric, phase status, score history |
| Audience | the working agent, right now | the team / future readers |

Do not copy long content into the tracker. Keep it to objective + next actions + open items, and link
the requirements doc for detail.

## Why this matters

Long sessions drift between requirements-doc updates. The live tracker keeps the current objective
explicit so the agent (and the developer watching) always knows what is being done right now and what
comes next, without re-reading the whole requirements doc.
