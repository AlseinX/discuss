---
name: discuss
description: "Use when the user explicitly asks to discuss, clarify, brainstorm, plan, enter plan mode, or work through a potentially complex task whose goal, scope, output, assumptions, or implementation details are not well defined. Use before implementation when requirements need clarification or when important choices are unspecified."
---

# Discuss

Guide a discussion that turns an incomplete idea into a well-defined target such as a definition, specification, implementation plan, document, or code change. Keep the main conversation focused on decisions and user intent.

## Start

Determine the discussion topic before anything else.

- If the user invoked the skill with an explicit `$` skill mention, slash-command form, namespaced plugin form such as `$discuss:discuss` or `/discuss:discuss`, or plan mode and also provided a description, use that description as the topic.
- If the user invoked the skill or entered plan mode without a description, ask what topic they want to discuss.
- If prior context contains one obvious topic, ask whether that is the intended topic; do not assume it without confirmation.
- If the skill triggered automatically, use the task that triggered it as the topic.

Identify the intended final output before detailed discussion:

- Ask what the user wants the discussion to produce, such as a definition document, specification, implementation plan, or code change.
- Ask where the output should go when the topic does not specify it.
- Do not continue into detailed decisions until the target output and destination are explicit.

## Discussion Workflow

Work breadth-first from high-level decisions toward detail.

1. List the current knowns and unknowns at the active level of detail.
2. Pick one unresolved point to discuss.
3. Ask for the user's intent on that point.
4. If the user cannot answer or asks for help, use subagents to investigate feasible options.
5. Present all reasonable options, a recommended priority order, and tradeoffs.
6. Ask the user to choose or revise the options.
7. After a choice is made, check for hard blockers, contradictions, or infeasible assumptions.
8. Record the decision in the conversation and move to the next unresolved point.

Prefer resolving all major points at the same level of precision before drilling into one area. Follow the user's requested order when they explicitly choose a different direction.

## Subagent Requirement

Use subagents for all exploration work, including web research, repository inspection, file reads, file writes, command execution, and validation runs.

- Keep the main agent focused on questions, decisions, and synthesis.
- Give each subagent the full strength, scope, and constraints of the user's request.
- Do not weaken words such as "all", "must", "complete", or "no omissions" when delegating.
- Ask subagents for concise evidence summaries rather than raw logs.
- If no native subagent mechanism is available, read `references/command-line-subagents.md` and use bash-invoked command-line agents as subagents.
- If neither native subagents nor command-line agent invocation is available, stop and tell the user that exploration is blocked by the missing subagent capability; ask whether to proceed without that constraint.

## Evidence Rules

Do not use guesses as decision inputs.

- Investigate factual claims before relying on them, unless the user explicitly and confidently states the fact.
- If repeated investigation cannot resolve a fact because of real constraints, state the investigated scope and limitation before labeling any remaining statement as a guess.
- Treat examples, test configs, local configs, or one run's selected options as evidence for that instance only, not as project scope or capability boundaries.

## Tradeoffs

Protect the user's explicit requirements first.

- Do not recommend an option that discards an explicit user requirement merely because another goal seems important.
- If an explicit requirement is unreasonable, contradictory, or proven impossible, explain why and ask the user how to revise it.
- Do not silently ignore or weaken explicit requirements.
- When presenting choices, include the practical cost, risk, and effect of each option.

## Completion

Do not decide the discussion is complete without the user's explicit confirmation.

- Compare the current decisions against the detail level required by the target output.
- When coverage appears complete, suggest that the discussion may be ready to finish and ask the user to confirm.
- Do not ask too early while material points remain unresolved.
- Do not continue into unnecessary detail beyond what the target output needs.

## Final Output

After the user confirms the discussion is complete, produce the requested output at the requested destination.

- Preserve every decision and relevant detail from the discussion.
- Exclude details that are beyond the required precision of the target output.
- Write only the current correct result; do not include process history, trial-and-error notes, or superseded conclusions.
- If the conversation compacted after the discussion began, read the complete discussion history from the discussion start through completion before writing the final output.
- Recover the entire discussion record without filtering, excluding, or truncating any conversation content from the discussion start through completion.
- If recovering history triggers another compaction, restart recovery from the discussion start and continue until the complete unfiltered discussion record is available.
- Do not write the final output unless the complete discussion record is available in context or has been recovered from history.
