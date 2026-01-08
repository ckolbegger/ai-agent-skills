# ai-agent-skills

This repo contains various skills that I have written for Claude Code.
All skills have been tested in Claude Code.
Most have also been tested with both OpenCode and Codex.

Here is a high-level description of each of the skills, in the order they were developed.

## soc-design-review

This skill is designed to be used with speckit to make sure that the design created  
in the planning step has clear boundaries between data, domain, and presentation layers.
It has some project-specific assumptions, so your mileage may vary.

## tdd-task

I created this skill for two reasons.

1 - I got tired of having to watch coding agents to make sure they were doing TDD.

2 - I noticed that a full suite test run could eat up quite a lot of the context window.

This skill is intended to be used by a subagent to implement a single task from a task breakdown file.
In Claude Code it can do this in a subagent, which minimizes the impact on the main agent loop context window.

I typically run it by telling Claude Code
Please start a subagent using the tdd-task skill to implement T006 from @spec-002/tasks.md.


## jit-task-audit

Often, when I have a coding agent create an implementation plan from a PRD
the AI likes to design things in layers and it writes stories that way.
So frequently there will be a "foundations" story that does all of the interfaces and database schema.
Then it would write a bunch of user stories, and usually the first user story
only uses a small part of what was created in the foundation stories.

This prompt will take a plan like that and move the foundation pieces out of
an initial foundation story and into the user stories where the actually will be used.
In practice, for a decent sized spec this cuts the number of tasks needed to implement
the first story at least in half.

Run this as:

Please start a subagent using the /jit-task-audit skill to review @spec-002/tasks.md

It will give you a list of suggestions and then you can tell it to apply the suggestions to the plan.



