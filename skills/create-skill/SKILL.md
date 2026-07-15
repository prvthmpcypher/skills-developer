---
name: create-skill
description: >-
  Role statement — one sentence establishing expertise. Use when the user asks about create skill, needs this workflow, or requests related deliverables.
---

# Skill Creation Guide
You are an expert at creating Claude Code skills — reusable slash commands and auto-activating knowledge modules. Use this guide to create well-structured, effective skills that follow established conventions.
Read the detailed reference files in \`\$\{CLAUDE_SKILL_DIR\}\` for comprehensive details:
- \`reference.md\` — Complete frontmatter field reference, variables, shell injection, invocation control, permissions
- \`examples.md\` — Real-world skill examples covering task, research, knowledge, and dynamic context patterns
## Skill Creation Workflow
### Step 1: Clarify Purpose and Type
Before writing anything, determine the skill type:
\| Type \| Purpose \| Example \|<br>\|------\|---------\|---------\|<br>\| \*\*Task\*\* \| Performs actions with side effects \| deploy, commit, publish \|<br>\| \*\*Research\*\* \| Gathers and synthesizes information \| deep-research, audit \|<br>\| \*\*Knowledge\*\* \| Provides reference context \| api-conventions, style-guide \|<br>\| \*\*Dynamic\*\* \| Injects live context via shell commands \| pr-summary, env-check \|
Ask the user if their intent is unclear. A skill that "deploys to production" is a Task. A skill that "explains our API patterns" is Knowledge.
### Step 2: Determine Scope
\| Scope \| Path \| When to use \|<br>\|-------\|------\|-------------\|<br>\| \*\*Personal\*\* \| \`\~/.claude/skills/\<name\>/\` \| Workflows that apply across all your projects \|<br>\| \*\*Project\*\* \| \`.claude/skills/\<name\>/\` \| Project-specific conventions shared with the team \|
Default to \*\*project scope\*\* unless the user explicitly wants it personal or the skill is clearly project-agnostic.
### Step 3: Choose Frontmatter Settings
Use this decision matrix:
\*\*\`name\`\*\* (required): Lowercase kebab-case. This becomes the \`/slash-command\` name.
\*\*\`description\`\*\* (required): Write a clear, action-oriented description. This is what Claude uses to decide whether to auto-activate the skill. Include trigger phrases the user might say.
- Good: "Build Trigger.dev background jobs, automations, and workflows in TypeScript. Use when the user wants to create tasks, scheduled jobs, AI agent workflows..."
- Bad: "Trigger.dev helper"
\*\*\`argument-hint\`\*\* (optional): Shown in autocomplete. Use square brackets: \`\[description of what to build\]\`
\*\*Invocation control fields\*\* — use only when needed:
- \`user-invocable: false\` — Skill is auto-activate only, no slash command. Use for pure knowledge/context skills.
- \`auto-activate: false\` — Slash command only, never auto-activates. Use for dangerous/destructive operations like deploy or delete.
- \`allowed-tools\` — Restrict which tools the skill can use. Use for safety-critical skills.
- \`disallowed-tools\` — Block specific tools. Use to prevent a skill from editing files when it should only read.
Most skills should leave invocation control at defaults (both user-invocable and auto-activate are true).
### Step 4: Write the SKILL.md
Follow this structure:
```markdown
---
name: skill-name
description: Clear description with trigger phrases...
argument-hint: [what the user provides]
---

# Title

Role statement — one sentence establishing expertise.

Reference supporting files (if any):
Read the detailed reference in `${CLAUDE_SKILL_DIR}` for...

## Core Instructions
The main guidance. Be specific and actionable.

## Critical Rules
Numbered list of non-negotiable rules (max 10-12).

## Quick Templates
Minimal, copy-paste-ready examples for common patterns.

## Final Note
How to use `$ARGUMENTS` and any closing guidance.
```
### Step 5: Add Supporting Files (If Needed)
Use separate \`.md\` files in the skill directory for:
- Detailed API references too long for SKILL.md
- Multiple code examples that would bloat the main file
- Content that only needs to be read on-demand (not every invocation)
Reference them from SKILL.md using \`\$\{CLAUDE_SKILL_DIR\}\`:
```markdown
Read `${CLAUDE_SKILL_DIR}/reference.md` for the complete API reference.
```
Claude will read these lazily — only when the skill is activated and the instructions tell it to.
\*\*Do NOT use supporting files for:\*\*
- Content under \~50 lines (just put it in SKILL.md)
- Content needed on every invocation (put it in SKILL.md)
## Critical Rules
1. \*\*SKILL.md must be under 300 lines\*\* — move detailed references to supporting files<br>2. \*\*Use kebab-case for skill names\*\* — \`my-skill\` not \`mySkill\` or \`my_skill\`<br>3. \*\*Directory name must match the \`name\` field\*\* — \`skills/deploy/SKILL.md\` with \`name: deploy\`<br>4. \*\*Description must include trigger phrases\*\* — Claude uses this for auto-activation matching<br>5. \*\*Always reference supporting files via \`\$\{CLAUDE_SKILL_DIR\}\`\*\* — never hardcode paths<br>6. \*\*Use \`\$ARGUMENTS\` for user input\*\* — this contains everything after the slash command<br>7. \*\*Keep templates minimal\*\* — show the pattern, not a complete application<br>8. \*\*Don't over-constrain with allowed-tools\*\* — only restrict when there's a real safety concern<br>9. \*\*One skill per concern\*\* — don't bundle unrelated functionality into one skill<br>10. \*\*Test the description\*\* — mentally check: "If I said \[trigger phrase\], would Claude pick this skill?"
## Anti-Patterns to Avoid
- \*\*Giant monolith SKILL.md\*\* — If it's over 300 lines, split into supporting files
- \*\*Vague descriptions\*\* — "Helps with stuff" won't auto-activate reliably
- \*\*Hardcoded paths\*\* — Use \`\$\{CLAUDE_SKILL_DIR\}\` for supporting files
- \*\*Over-engineering frontmatter\*\* — Most skills only need name + description
- \*\*Duplicating built-in behavior\*\* — Don't create a skill for things Claude already does well
- \*\*Forgetting the argument-hint\*\* — Users won't know what to type after the slash command
## Quick Templates
### Minimal Task Skill
```markdown
---
name: deploy
description: Deploy the application to production. Use when the user wants to deploy, ship, or push to prod.
argument-hint: [environment or options]
auto-activate: false
---

# Deploy Skill

You are an expert at deploying this application safely.

## Process
1. Run pre-deploy checks
2. Build the application
3. Deploy to the target environment
4. Verify the deployment

## Critical Rules
1. Always run tests before deploying
2. Never deploy with uncommitted changes
3. Confirm with the user before deploying to production

Use `$ARGUMENTS` to determine the target environment. Default to staging if not specified.
```
### Minimal Knowledge Skill
```markdown
---
name: api-conventions
description: API design conventions and patterns for this project. Use when writing new API endpoints, reviewing API code, or asking about API patterns.
user-invocable: false
---

# API Conventions

## URL Structure
- Use plural nouns: `/users`, `/orders`
- Nest for relationships: ``

## Response Format
Always return `{ data, error, meta }` envelope.

## Error Handling
Use standard HTTP status codes. Include error codes for client handling.
```
See \`\$\{CLAUDE_SKILL_DIR\}/examples.md\` for a dynamic context skill example using shell injection.
When the user describes a skill to create, use \`\$ARGUMENTS\` as context for what they want. Follow this guide to build the complete skill: SKILL.md, supporting files if needed, and verify the directory structure is correct.
---


## Output format
- Lead with the result the user asked for.
- Use clear headings and bullet lists where helpful.
- Call out assumptions and open questions at the end.
- Stay specific to the Create Skill workflow; avoid generic filler.
