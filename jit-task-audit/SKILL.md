---
name: jit-task-audit
description: >
  Audit implementation tasks for premature implementation using Just-In-Time (JIT) principles — identifying code built before it's actually needed and recommending deferral to the story that first requires it. Invoke this skill proactively whenever the user shares a tasks.md, implementation plan, or phase breakdown, or immediately after a task breakdown has been generated in the conversation, especially when you see a "Setup" or "Foundation" phase, utility code with no current callers, or any plan that looks front-loaded. Don't wait to be asked — if someone is about to start implementing a multi-phase plan, offer this audit.
argument-hint: [path-to-tasks-file]
context: fork
agent: general-purpose
effort: high
---

# Just-In-Time Task Audit Skill

> **Run this skill in a subagent.** Spawn a subagent with the tasks file and this skill, perform the audit there, then return only the recommendations and JIT score to the main conversation. This keeps the main context window clean. *(On Claude Code this is enforced automatically via `context: fork`. On other CLIs, please spawn a subagent manually.)*

## Purpose

Apply Just-In-Time (JIT) auditing to implementation tasks to identify code being built before it's actually needed. JIT implementation means:

- Only build what the current user story needs
- Defer utilities, calculations, and types until they're immediately required
- Avoid "just in case" code or anticipatory implementations

## Finding the Tasks File

1. **Argument** — if a path was provided as an argument, use that.
2. **Current conversation context** — if the conversation references a specific tasks file (by name or path), use that.
3. **Common filenames in the current directory** — look for `tasks.md`, `TODO.md`, `TASKS.md`, or any `.md` file whose name suggests a task breakdown.
4. **Ask** — if nothing is found, ask the user to point you to the file before proceeding.

## Execution Flow

1. **Read the tasks** - Load the tasks file
2. **Apply audit questions** from `audit-questions.md`
3. **Map dependencies** - Track which story introduces each piece of infrastructure
4. **Identify premature items** using `patterns.md`
5. **Generate recommendations** with specific move/defer actions
6. **Calculate JIT score** - percentage of items correctly placed

## Output Format

After completing the audit, produce a **JIT Audit Report**:

```markdown
## JIT Audit Results

### Items Correctly Placed
| Item | Built In | Used In | Status |
|------|----------|---------|--------|
| [name] | [phase] | [phase] | OK |

### Premature Implementations Found
| Item | Built In | Needed In | Recommendation |
|------|----------|-----------|----------------|
| [name] | [phase] | [phase] | [action] |

### Just-In-Time Score
X/Y items follow JIT principle (Z%)

### Recommendations
1. Move [item] from [early phase] to [correct phase]
2. Remove [item] entirely, implement inline when needed
3. Keep [item] - it's needed by [current story]
4. Convert [item] to minimal inline version in [story]
```

## When Everything Is Already JIT-Compliant

If the audit finds no premature implementations, still show the "Items Correctly Placed" table and briefly explain why each item passes. A 100% score is a meaningful result — don't skip the work, just skip the warnings.

## Key Principles

1. **Build only what the current user story needs**
2. **Defer everything else** - you know more after completing each story
3. **Inline first, extract later** - don't build frameworks upfront
4. **Types evolve with implementation** - extend when actually used
5. **No "just in case" code** - if you can't name the caller now, defer it

