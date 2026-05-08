---
name: issue-driven-development
description: Starting workflow for agents to pick up and work on the next open GitHub Issue. Use when the user asks to start work, work on the next issue, pick up a ready issue, or continue issue-driven development.
---

# Work Next Issue

Use this skill as the mandatory starting point before touching code for an issue-driven session.

## Before Starting (Mandatory)

1. Run `./agent-init.sh` and verify it finishes without errors.
   - If it fails, stop and fix the environment before touching application code.
2. Read GitHub Issues with `gh issue list` to understand the state of the last session.
3. Choose exactly one task with status pending. Do not work on more than one issue at a time.

## Choosing the Task

1. List all open GitHub Issues:
   ```bash
   gh issue list
   ```
2. Filter issues by the `ready-for-work` label.
3. Choose the issue with the lowest issue number/id.
4. Add the `in-progress` label to that issue before making code changes:
   ```bash
   gh issue edit <issue-number> --add-label in-progress
   ```
5. Document in the GitHub Issue that you are starting work and summarize the intended first step.

## Parent PR Workflow

- Do not create a separate PR for each child issue.
- The PR is created for the parent issue: the GitHub Issue with the `parent` label.
- Each child issue should produce one focused commit inside the parent issue PR.
- If a parent PR already exists, continue working on that PR branch and add the child issue commit there.
- If no parent PR exists yet, create one for the parent issue when the first child issue needs review.
- Child issue commits must use Conventional Commits format and should reference the child issue when useful, e.g. `feat: add brewfile parser (#4)`.

## GitHub Writing Footer

When writing any GitHub Issue comment, GitHub Issue description, or PR description, always end the body with this italic footer:

```markdown

_Written by agent_
```

There must be exactly one blank line between the final sentence of the message and `_Written by agent_`.

Example:

```markdown
Implemented the initial CLI skeleton and verified the command paths.

_Written by agent_
```

## Hard Rules (Non-Negotiable)

- Work on only one child issue at a time. Do not mix changes from multiple child issues in the same commit.
- Do not declare a task done without green tests.
- Run `./agent-init.sh` before finishing and ensure the test block passes 100%.
- Document progress in the GitHub Issue while you work, not only at the end.
- Leave the repository clean before closing the session.
- All commits created while working the issue must use Conventional Commits format, e.g. `feat: add brewfile parser`, `fix: resolve sync path precedence`, `test: cover capture reconciliation`.
- Do not create child-issue PRs; add child-issue commits to the parent issue PR instead.
- If you do not know something, check `docs/` first when it exists. Do not make up project behavior.
- All GitHub Issue comments, Issue descriptions, and PR descriptions written by the agent must include the `_Written by agent_` footer with one blank line before it.

## While Working

- Keep the GitHub Issue updated with meaningful progress notes, blockers, decisions, and test results.
- If docs exist for the relevant area, read them before implementing.
- Avoid broad refactors or unrelated cleanups unless they are required for the selected issue.
- Do not introduce temporary files, debug prints, or TODOs without context.

## If You Get Stuck

1. Re-read the relevant section in `docs/`, if present.
2. If a tool or project command does not behave as expected, do not invent a workaround.
3. Document the issue, command output, and current status in the GitHub Issue.
4. End the session cleanly instead of making speculative changes.

## Before Finishing

1. Run:
   ```bash
   ./agent-init.sh
   ```
   Everything must be green.
2. If the task is finished:
   - Add the `pending-review` label.
   - Remove the `in-progress` label.
   ```bash
   gh issue edit <issue-number> --add-label pending-review --remove-label in-progress
   ```
3. Link the parent PR to the child issue, if a parent PR exists or was created.
4. Confirm any commits created by the agent use Conventional Commits format and that the child issue's work is contained in a focused commit in the parent PR.
5. Ensure the repository is clean and contains no temporary files, debug prints, or unexplained TODOs.
6. Add a final GitHub Issue comment summarizing:
   - What changed
   - Tests run
   - Parent PR link, if applicable
   - Any follow-up needed
7. Ensure the final comment ends with the required footer:
   ```markdown

   _Written by agent_
   ```
