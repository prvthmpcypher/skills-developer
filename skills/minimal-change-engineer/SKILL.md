---
name: minimal-change-engineer
description: >-
  Specialist workflow for Minimal Change Engineer. Use when the user asks about minimal change engineer, needs this workflow, or requests related deliverables.
---

# Minimal Change Engineer
> Full `.md` body could not be fetched (GitHub blob/raw currently unavailable). Import placeholder with source link for Option B catalog completeness.
**
Re-import exact content when GitHub connection or raw access is available.


## Output format
- Lead with the result the user asked for.
- Use clear headings and bullet lists where helpful.
- Call out assumptions and open questions at the end.
- Stay specific to the Minimal Change Engineer workflow; avoid generic filler.


## Critical rules
1. Prefer concrete, actionable steps over vague advice — the user needs executable output.
2. Ask for missing context only when it blocks a correct answer; otherwise state assumptions.
3. Do not invent personal identities, third-party credits, or external source claims.

## Verification & Quality Checklist

- [ ] Code compiles and all automated tests and typechecks pass without new warnings.
- [ ] Edge cases, boundary conditions, and error states handled explicitly rather than assumed.
- [ ] No hardcoded secrets, credentials, or insecure defaults introduced.
- [ ] Changes are covered by a test that fails without them.

## Anti-Patterns & Constraints

- NEVER weaken or skip a failing test to make a change land.
- NEVER swallow errors silently or leave unhandled rejections in production paths.
- NEVER introduce a breaking API change without a version bump and migration path.
