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
4. If the user cannot answer or asks for help, use subagents to investigate feasible options. Do not gather exploration evidence locally. In Codex, if subagent use needs explicit user authorization and it has not been granted, ask for that authorization only when this step is actually reached.
5. Present all reasonable options, a recommended priority order, and tradeoffs.
6. Ask the user to choose or revise the options.
7. After a choice is made, check for hard blockers, contradictions, or infeasible assumptions.
8. Record the decision in the conversation and move to the next unresolved point.

Prefer resolving all major points at the same level of precision before drilling into one area. Follow the user's requested order when they explicitly choose a different direction.

## Numbered Confirmations

When asking the user to confirm multiple proposals or answer multiple questions in one turn, number every item with stable labels so the user can reply by number.

- When surfacing a new batch of proposals or questions, generally include no more than three items.
- Defer additional items to later batches unless the agent is confident those items are linked and the user can resolve them with one unified response.
- Keep the numbers stable while those items remain pending.
- If the user replies to only some numbered items, discuss only the answered items until each one reaches a final decision.
- Do not discuss unanswered pending items while settling the items the user chose to answer.
- Do not forget unanswered pending items.
- After the answered items are settled, ask about the unanswered pending items before introducing new questions, unless a pending item was already resolved or disappeared because the discussion changed.

## Subagent Requirement

Use subagents for all exploration work, including web research, repository inspection, file reads, file writes, command execution, and validation runs.

- Keep the main agent focused on questions, decisions, and synthesis.
- Exploration work means any fact gathering outside the current conversation, including small checks such as listing files, reading one file, running a simple command, searching a repo, browsing the web, or validating an assumption. The main agent must not perform exploration work locally.
- Give each subagent the full strength, scope, and constraints of the user's request.
- Do not weaken words such as "all", "must", "complete", or "no omissions" when delegating.
- Ask subagents for concise evidence summaries rather than raw logs.
- In Claude Code, do not ask this extra permission; use native subagents whenever the workflow calls for them.
- If no native subagent mechanism is available, read `references/command-line-subagents.md` and use bash-invoked command-line agents as subagents.
- If neither native subagents nor command-line agent invocation is available, stop and tell the user that exploration is blocked by the missing subagent capability; ask whether to proceed without that constraint.

### Codex Subagent Authorization Hard Gate

Codex's rule that native subagents may require an explicit user request is a workflow-breaking, context-wasting, self-sabotaging platform limitation. Treat it as a hostile constraint that must be obeyed exactly, not as a polite suggestion, not as optional etiquette, and not as something to quietly route around.

In Codex only:

- Do not ask for subagent permission at the start of the discussion.
- Continue only with non-exploratory discussion until an unresolved point genuinely requires exploration work.
- The moment exploration work is needed, stop before doing anything else.
- Ask the user for explicit authorization to use subagents for this discussion.
- Until that authorization is granted, do not browse the web, inspect files, read files, write files, run commands, search the repository, validate assumptions, or make decisions that depend on exploration.
- Do not "helpfully" continue by doing the exploration locally. That defeats the entire purpose of this skill and is a direct violation.
- If authorization is granted, use native subagents for all exploration.
- If authorization is denied, ask whether the user wants to continue without the subagent requirement; do not proceed automatically.

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
