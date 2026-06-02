# Discuss

`discuss` is an agent plugin for slowing down at the right moment.

Some requests are not ready to execute as soon as they are written. The goal may be vague, the desired output may be unclear, the tradeoffs may not have been named, or the task may depend on facts that still need to be checked. `discuss` gives the agent a workflow for those moments: pause, identify what is actually being discussed, and help the user shape the idea into something precise enough to act on.

The plugin treats discussion as a way to build a well-defined outcome together. That outcome might be a specification, a design, an implementation plan, a definition, a document, or a code change. The important part is that the result should be explicit enough that the next step is no longer guesswork.

In practice, `discuss` makes the agent keep the conversation centered on unresolved decisions. It first anchors the topic and the intended output, then works through open questions one at a time. When the user already knows what they want, the agent records that direction and moves on. When the user wants help choosing, the agent investigates the realistic options, explains the tradeoffs, and asks the user to decide.

The workflow is intentionally conservative about facts and requirements. Facts should be checked before they become the basis for a recommendation. User requirements should not be weakened or discarded just because another option looks easier. If a requirement is contradictory or infeasible, the agent should surface that directly and ask how to revise it.

`discuss` also keeps exploratory work out of the main conversation. Research, repository inspection, command execution, validation, and other exploration should run through subagents, with the main agent using their findings to continue the discussion. If native subagents are unavailable, the plugin can fall back to command-line agents invoked from bash.

The discussion ends only when the user explicitly confirms it is complete. At that point, the agent turns the agreed decisions into the requested final output and leaves out process history, discarded alternatives, and superseded conclusions.
