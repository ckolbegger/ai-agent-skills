---
name: handoff
description: >
  Generate a comprehensive handoff document that captures the current state of work for continuity across coding sessions — covering the goal, progress, what worked, what failed, key decisions, open questions, and next steps. This is a manual-only skill: run it when you explicitly ask, typically before ending a session, before resetting a long context, or when switching between major phases of work. It depends on the current conversation, so it must run in the main context (never in a subagent).
argument-hint: [output-path]
disable-model-invocation: true
effort: xhigh
---

# Handoff Document Generator

Generate a comprehensive handoff document for continuity across coding sessions.

> **Run this skill in the main conversation, not a subagent.** It synthesizes the handoff primarily from the current conversation history (goal, decisions, what worked/failed, open questions). A subagent would start with a clean context and miss all of that, so this skill must run inline.

## Usage

```
/handoff [output-path]
```

- `output-path`: Optional. Defaults to `handoff.md` in the current directory, or alongside the relevant feature/spec files if the project organizes work that way.

## Execution Steps

1. **Determine output path**:
   - Use the provided path if given
   - If the work lives in a dedicated feature/spec directory, write `handoff.md` there
   - Otherwise use `handoff.md` in the current directory

2. **Gather context**:
   - Get current git branch: `git branch --show-current`
   - Check branch push status: `git status` and `git log origin/BRANCH..HEAD`
   - List modified/staged files: `git status --short`
   - Check for any tags: `git tag --points-at HEAD`
   - Look for planning/spec artifacts the project uses (e.g. spec, plan, research, design, or task files) and reference whatever exists

3. **Identify the work in scope**:
   - Derive the feature(s) being worked on from the conversation history and git status (branch name, recent commits, modified files) — don't assume any particular framework or directory layout
   - A session may touch more than one feature; capture each

4. **Extract information from conversation history**:
   - Goal/purpose of the work
   - Phases completed (e.g. Spec, Plan, Research, Implementation)
   - Approaches that worked well
   - Approaches that failed or were rejected
   - Key technology/architecture decisions made
   - Open questions or blockers
   - Next steps discussed

5. **Generate handoff document** using the template below

6. **Write file** and confirm to user

## Output Template

```markdown
# Handoff Summary: [Feature Name]

**Date**: [YYYY-MM-DD]
**Branch**: `[branch-name]`

---

## Goal

[2-3 sentence description of what the project is trying to accomplish. Include key user-facing features.]

---

## Current Progress

| Phase | Status | Artifacts |
|-------|--------|-----------|
| **Spec** | [Complete/In Progress/Not Started] | [path to spec artifact, if any] |
| **Plan** | [Complete/In Progress/Not Started] | [path to plan artifact, if any] |
| **Research** | [Complete/In Progress/Not Started] | [path to research artifact, if any] |
| **Implementation** | [Complete/In Progress/Not Started] | [key files] |

[Brief description of what was accomplished in this session]

---

## What Worked

| Approach | Outcome |
|----------|---------|
| [Specific approach taken] | [Why it worked / result] |
| [Specific approach taken] | [Why it worked / result] |

## What Failed / Avoid

| Approach | Reason |
|----------|--------|
| [Approach considered/rejected] | [Why it was rejected] |
| [Approach tried that failed] | [What went wrong] |

---

## Next Steps

1. **[Task Name]** - [Specific action with exact file path]
2. **[Task Name]** - [Specific action with exact file path]
3. **[Task Name]** - [Specific action with exact file path]

---

## Branch Info

| Property | Value |
|----------|-------|
| **Branch** | `[branch-name]` |
| **Status** | [pushed to origin / local only] |
| **Tag** | `[tag-name or N/A]` |
| **Uncommitted Changes** | [list any or "None"] |

**Key Files**:
- `[file path]` - [brief description]
- `[file path]` - [brief description]

---

## Key Decisions

| Area | Decision |
|------|----------|
| **[Category]** | [Choice made] |
| **[Category]** | [Choice made] |

---

## Open Questions

- [Question that needs resolution before proceeding]
- [Question that needs resolution before proceeding]

[Or: "None blocking. Ready to proceed with [next phase]."]
```

## Formatting Rules

- Use tables for: Current Progress, What Worked, What Failed, Branch Info, Key Decisions
- Be specific with file paths in Next Steps (e.g., "Create `specs/001-feature/data-model.md`")
- Mark blocking issues clearly in Open Questions with "**BLOCKING:**" prefix
- If no open questions, explicitly state "None blocking"
- Include relative file paths from repo root
- Keep Goal section concise (2-3 sentences max)
