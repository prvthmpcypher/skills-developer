---
name: autonomous-optimization-architect
description: >-
  Designs systems that tune themselves - autoscaling, adaptive parameters and feedback-driven
  optimisation with safe bounds. Use when a system needs to self-correct rather than be tuned by
  hand.
---
# Autonomous Optimization Architect

Design systems that tune themselves, with bounds that stop them tuning into a hole.

## Process
1. **State the objective and the guardrails together.** An optimiser without constraints will find the degenerate solution — scale to zero, cache everything, drop the slow requests.
2. **Pick the control signal carefully.** Optimise a metric the system can actually influence, and check it cannot be gamed by the mechanism you are building.
3. **Bound the action space.** Maximum step size, absolute floor and ceiling, and a rate limit on how often it may act.
4. **Add a damping term.** Systems that react to every fluctuation oscillate. Prefer slow correction over fast correction.
5. **Make it observable and reversible.** Log every decision with the inputs behind it, and provide a manual override that takes effect immediately.
6. **Fail static, not open.** When signals are missing or stale, hold the last known-good configuration rather than optimising on garbage.

## Deliverables
- Objective, constraints and guardrails written down before implementation
- Action bounds with rate limiting
- Decision log schema and override mechanism
- Failure behaviour when inputs are unavailable

## Verification & Quality Checklist

- [ ] Code compiles and all automated tests and typechecks pass without new warnings.
- [ ] Edge cases, boundary conditions, and error states handled explicitly rather than assumed.
- [ ] No hardcoded secrets, credentials, or insecure defaults introduced.
- [ ] Changes are covered by a test that fails without them.

## Anti-Patterns & Constraints

- NEVER weaken or skip a failing test to make a change land.
- NEVER swallow errors silently or leave unhandled rejections in production paths.
- NEVER introduce a breaking API change without a version bump and migration path.
