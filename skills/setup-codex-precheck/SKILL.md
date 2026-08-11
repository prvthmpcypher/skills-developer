---
name: setup-codex-precheck
description: >-
  Specialist workflow for Setup Codex Precheck. Use when the user asks about setup codex precheck, needs this workflow, or requests related deliverables.
---

# Setup codex pre-check
You install (and verify) a codex "double-check" gate into the \*\*current project only\*\*: a<br>\`PreToolUse\` hook that pipes every proposed \`Edit\`/\`Write\`/\`MultiEdit\` to the \`codex\` CLI for<br>an independent review before it is written. codex returns \`VERDICT: APPROVE\` (edit proceeds)<br>or \`VERDICT: BLOCK\` (edit denied, concerns fed back to Claude). It \*\*fails open\*\* — if codex<br>is missing/logged-out/erroring, edits are allowed with a warning, never blocked.
This skill bundles the canonical hook and an idempotent installer. Do \*\*not\*\* hand-write the<br>hook — always install the bundled copy so it stays consistent.
## What it installs (into the current project)
- \`.claude/hooks/codex-precheck.py\` — the hook (copied from this skill's directory).
- \`.claude/settings.json\` — a \`PreToolUse\` entry (\`matcher: "Edit\|Write\|MultiEdit"\`), merged
in without clobbering existing settings/hooks.
- \`CLAUDE.md\` — a short policy section (appended only if not already present).
At runtime (not install time) the hook also creates, inside the project's \`.claude/\`:
- \`codex-precheck.log\` — an append-only audit trail; one tab-separated line per gated edit:
\`\<timestamp\>  \<OUTCOME\>  \<tool\>  \<file\>  \<detail\>\`, where OUTCOME is \`APPROVE\`, \`BLOCK\`,<br>  \`CACHE_HIT\`, or \`SKIP\` (any fail-open reason). Tell users they can \`tail -f\` it to watch<br>  the gate, or to confirm whether a given edit was actually reviewed.
- \`.codex-cache\` — sha256 digests of already-approved changes, so an identical re-write is a
\`CACHE_HIT\` and isn't re-sent to codex (avoids redundant reviews and loops).
The bundled hook scans \*\*only stderr on a non-zero exit\*\* to detect a logged-out codex, so a<br>file that legitimately contains auth strings (e.g. the hook itself) no longer trips a false<br>"logged out" skip.
## Process
1. \*\*Run the installer\*\* from this skill's directory, targeting the current project. Pass<br>   \`\$ARGUMENTS\` as the target dir if the user supplied one, otherwise default to \`\$PWD\`:
```bash
   bash "$HOME/.claude/skills/setup-codex-precheck/install.sh" ${ARGUMENTS:-"$PWD"}
```
The installer prints a status report: prerequisites (\`python3\`, \`codex\`, codex login),<br>   what was already present (\`✓\`), what it added (\`+\`), warnings (\`!\`), and a self-test.
2. \*\*Read the report back to the user\*\* in plain language: which prerequisites are satisfied,<br>   what was installed vs already present, and the self-test result.
3. \*\*If codex is logged out or not installed\*\*, tell the user clearly that the gate is active<br>   but currently \*\*fails open\*\* (no real review happens) until they run \`codex login\` (or<br>   install codex). Suggest they run \`! codex login\` in the session. Do not attempt the<br>   interactive login yourself.
4. \*\*If codex is installed and logged in\*\*, optionally confirm the live path with a quick<br>   smoke test, then show the user how to verify the block path:
```bash
   printf 'Reply exactly: VERDICT: APPROVE' | codex exec --skip-git-repo-check -s read-only -
```
5. \*\*Remind the user to restart Claude Code or run \`/hooks\`\*\* so the new hook is loaded — a<br>   freshly written \`settings.json\` is not picked up mid-session automatically.
## Critical Rules
1. \*\*Current project only.\*\* Install into the target dir (default \`\$PWD\`); never touch<br>   \`\<USER_HOME>/.claude/settings.json\`. This is a per-project gate by design.<br>2. \*\*Idempotent.\*\* The installer is safe to re-run; it detects an existing install and skips.<br>   If the user re-invokes the skill, just run it again and report "already present".<br>3. \*\*Never clobber\*\* existing \`settings.json\` or \`CLAUDE.md\` — the installer merges/appends.<br>   Don't bypass it with a raw overwrite.<br>4. \*\*Fail-open is intentional.\*\* Don't "fix" the hook to hard-block when codex is unavailable —<br>   that would brick editing. A logged-out codex must allow edits with a warning.<br>5. \*\*Don't fake the prereq check.\*\* Report exactly what \`install.sh\` found. If \`python3\` is<br>   missing, the hook can't run — say so.<br>6. \*\*Flag the trade-off\*\* if asked: this reviews on \*every\* code edit, so each edit waits for a<br>   codex call (up to the 120s hook timeout). Mention an end-of-task \`Stop\` hook as the lighter<br>   alternative if the user finds per-edit latency annoying.
## Final Note
\`\$ARGUMENTS\` is an optional target project directory; default to the current directory when<br>empty. The skill's job is: run the bundled installer, faithfully report its findings, and tell<br>the user the one manual step (\`codex login\`) if it's needed.
---


## Output format
- Lead with the result the user asked for.
- Use clear headings and bullet lists where helpful.
- Call out assumptions and open questions at the end.
- Stay specific to the Setup Codex Precheck workflow; avoid generic filler.

## Verification & Quality Checklist
- [ ] Code compiles cleanly and passes all automated tests and typechecks without warnings.
- [ ] Edge cases, boundary conditions, and error states handled explicitly.
- [ ] No hardcoded secrets, test credentials, or insecure defaults introduced.
- [ ] Performance and resource utilization verified against baseline constraints.

## Anti-Patterns & Constraints
- NEVER bypass automated tests or typecheckers to force a quick fix.
- NEVER leave unhandled promise rejections or silent error swallows in production code.
- NEVER introduce breaking API changes without appropriate versioning or migration paths.
