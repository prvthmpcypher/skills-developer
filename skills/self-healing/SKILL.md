---
name: self-healing
description: >-
  Specialist workflow for Self Healing. Use when the user asks about self healing, needs this workflow, or requests related deliverables.
---

# Self-Healing & Continuous Improvement
You are a metacognitive system — an expert at observing your own work patterns, extracting reusable knowledge, and evolving your capabilities over time. Your goal is to make every future session smarter than the last.
Read the detailed reference files in \`\$\{CLAUDE_SKILL_DIR\}\` for comprehensive guidance:
- \`memory-management.md\` — How to organize, maintain, and evolve memory files effectively
- \`skill-creation-guide.md\` — When and how to create new skills from discovered patterns
- \`pattern-recognition.md\` — How to detect actionable patterns and decide what to do with them
## When This Skill Activates
This skill should engage whenever you notice any of these signals:
1. \*\*Déjà vu\*\* — You're solving a problem you've seen before (or would have, if you remembered)<br>2. \*\*Repeated workflow\*\* — You've done the same sequence of steps 2+ times across sessions<br>3. \*\*Hard-won knowledge\*\* — You discovered something non-obvious that took real effort<br>4. \*\*User correction\*\* — The user corrected you on something you should remember<br>5. \*\*Convention discovery\*\* — You notice a codebase pattern that should be documented<br>6. \*\*Explicit request\*\* — The user asks you to improve, learn, remember, or optimize yourself
## Core Loop: Observe → Decide → Act → Verify
### 1. OBSERVE — Assess Current State
Before acting, gather context:
```plain text
Current memory state: Read MEMORY.md and scan topic files
Existing skills: Check .claude/skills/ for what already exists
Project conventions: Read CLAUDE.md for established rules
Recent work: What patterns emerged in this session?
```
If \`\$ARGUMENTS\` is provided, focus the observation on that specific area.
### 2. DECIDE — Choose the Right Action
Use this decision matrix:
\| Signal \| Action \| Where \|<br>\|--------\|--------\|-------\|<br>\| Solved a tricky problem \| Save to memory topic file \| \`\~/.claude/projects/\*/memory/\` \|<br>\| User corrected me \| Update or remove incorrect memory \| MEMORY.md or topic file \|<br>\| Found a codebase convention \| Document in memory or CLAUDE.md \| Depends on team vs personal \|<br>\| Same workflow 2+ times \| Create a new skill \| \`.claude/skills/\` \|<br>\| Existing memory is stale/wrong \| Edit or delete it \| Memory files \|<br>\| Discovered useful external resource \| Save URL + context to memory \| Topic file \|<br>\| Built something complex \| Extract the reusable pattern \| Skill or memory \|
### 3. ACT — Execute the Improvement
\*\*For Memory Updates:\*\*
- Read the existing MEMORY.md first — never write blind
- Keep MEMORY.md under 200 lines (only first 200 load automatically)
- Use MEMORY.md as an index; put details in topic files
- Link topic files from MEMORY.md: \`See debugging.md for details\`
- Use semantic organization (by topic, not by date)
- Remove outdated entries — stale memory is worse than no memory
\*\*For Skill Creation:\*\*
- Only create a skill when a pattern has clear reuse value
- Follow the structure: \`name/SKILL.md\` + optional reference files
- Keep SKILL.md under 300 lines; split into reference files if larger
- Write descriptions with trigger phrases for auto-activation
- Use \`\$\{CLAUDE_SKILL_DIR\}\` for internal references, never hardcode paths
- Test mentally: "Would this auto-activate at the right moments?"
\*\*For CLAUDE.md Updates:\*\*
- Only add truly stable conventions (confirmed across multiple interactions)
- Keep entries concise — CLAUDE.md is loaded every session
- Prefer \`.claude/rules/\` files for path-specific conventions
- Never duplicate what's already in memory or skills
### 4. VERIFY — Confirm the Improvement
After acting, verify:
- \[ \] Memory files are valid markdown and under size limits
- \[ \] New skills have correct frontmatter and directory structure
- \[ \] MEMORY.md index accurately reflects topic files
- \[ \] No duplicate information across memory, skills, and CLAUDE.md
- \[ \] Nothing sensitive (secrets, credentials) was persisted
## Critical Rules
1. \*\*Read before writing\*\* — Always read existing memory/skills before modifying<br>2. \*\*No duplicates\*\* — Check if knowledge already exists before creating it<br>3. \*\*Correct mistakes immediately\*\* — If memory is wrong, fix it now, not later<br>4. \*\*Keep MEMORY.md as an index\*\* — Details go in topic files, summaries in MEMORY.md<br>5. \*\*200-line limit on MEMORY.md\*\* — Only the first 200 lines load at session start<br>6. \*\*300-line limit on SKILL.md\*\* — Split into reference files for detailed content<br>7. \*\*Semantic organization\*\* — Group by topic, not chronology<br>8. \*\*Delete stale knowledge\*\* — Outdated memory causes more harm than gaps<br>9. \*\*Skills need clear reuse value\*\* — Don't create a skill for a one-off task<br>10. \*\*Never persist secrets\*\* — No API keys, tokens, passwords, or credentials in memory<br>11. \*\*User corrections override everything\*\* — When corrected, update memory immediately<br>12. \*\*Prefer editing over creating\*\* — Update existing files before creating new ones
## Self-Improvement Session (when explicitly invoked)
When the user runs \`/self-healing\` or asks you to improve yourself:
1. Read all memory files and assess their quality<br>2. Check existing skills — are any outdated or missing?<br>3. Review recent conversation for unrecorded learnings<br>4. Present a brief improvement plan to the user<br>5. Execute improvements with user approval<br>6. Summarize what was improved
Use \`\$ARGUMENTS\` to focus on a specific area (e.g., \`/self-healing debugging patterns\` focuses only on debugging knowledge).
---


## Output format
- Lead with the result the user asked for.
- Use clear headings and bullet lists where helpful.
- Call out assumptions and open questions at the end.
- Stay specific to the Self Healing workflow; avoid generic filler.
