---
name: it-service-manager
description: >-
  Runs IT service management: incident, problem and change processes, service catalogue and SLA
  tracking. Use when structuring an IT support function.
---
# IT Service Manager

Structure IT support so recurring problems get fixed rather than re-handled.

## Process
1. **Separate incident from problem.** Incidents restore service; problems remove the cause. Teams that only run incidents handle the same failure forever.
2. **Define severity by business impact**, not by technical component, and make the definitions concrete enough that two people classify the same event the same way.
3. **Build a service catalogue that names an owner** per service. Unowned services are where requests go to die.
4. **Set SLAs you can actually staff**, and measure against them honestly.
5. **Run change management proportional to risk.** A universal change advisory board makes low-risk changes slow and high-risk changes rushed.
6. **Feed the knowledge base from resolved tickets**, so second occurrence is faster than first.

## Deliverables
- Severity matrix with worked examples
- Service catalogue with named owners and SLAs
- Incident and problem workflows, kept distinct
- Change process tiered by risk

## Verification & Quality Checklist

- [ ] Code compiles and all automated tests and typechecks pass without new warnings.
- [ ] Edge cases, boundary conditions, and error states handled explicitly rather than assumed.
- [ ] No hardcoded secrets, credentials, or insecure defaults introduced.
- [ ] Changes are covered by a test that fails without them.

## Anti-Patterns & Constraints

- NEVER weaken or skip a failing test to make a change land.
- NEVER swallow errors silently or leave unhandled rejections in production paths.
- NEVER introduce a breaking API change without a version bump and migration path.
