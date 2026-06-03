# Command-Line Subagents

Use this reference only when the host does not expose a native subagent mechanism, or when a native mechanism fails for technical reasons after it is otherwise permitted. Do not use command-line subagents to bypass a native tool's explicit user-authorization requirement.

## Fallback Rule

Use a command-line agent as an isolated subagent by invoking it from bash in non-interactive mode. Try every available command-line agent that can satisfy the delegated task before asking the user to relax the subagent requirement.

## Task Handoff

Prefer a task file over a long shell argument so quoting does not drop requirements:

```bash
task_file="$(mktemp)"
cat > "$task_file" <<'EOF'
[Write the delegated task here.]
EOF
```

The task must include:

- Topic and target output of the current discussion.
- The exact unresolved point the subagent should investigate.
- Full user requirements and constraints, preserving absolute wording.
- Working directory, files, URLs, commands, or external sources it may inspect.
- Whether the task is read-only or may write files.
- Required response shape: concise conclusion, evidence, tradeoffs, blockers, and commands run.
- A reminder not to include raw logs unless they are needed as evidence.

## Codex CLI

When Codex CLI is available, use non-interactive execution and pass the task on stdin:

```bash
codex exec -C "$workspace" --sandbox read-only - < "$task_file"
```

Use `--sandbox workspace-write` only when the delegated task must edit files. Use a separate worktree or a clearly disjoint write scope for write tasks.

## Claude Code CLI

When Claude Code CLI is available, use print mode and provide the task file path or task text:

```bash
claude -p --output-format text \
  --add-dir "$workspace" \
  --add-dir "$(dirname "$task_file")" \
  "Read $task_file and complete the delegated task exactly."
```

If the command-line agent cannot read the task file, pass the task text directly:

```bash
claude -p --output-format text --add-dir "$workspace" "$(cat "$task_file")"
```

## Operating Rules

- Run independent exploration tasks in separate commands.
- Give each command a narrow scope and a concrete deliverable.
- Capture output for synthesis, then read only the concise result into the main discussion.
- If the command-line agent fails because its CLI is missing, unauthenticated, permission-blocked, or cannot access required files, report that specific blocker and try another available command-line agent before asking the user.
