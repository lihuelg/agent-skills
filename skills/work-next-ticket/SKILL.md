---
name: work-next-ticket
description: Starting workflow for agents to pick up and work on the next ready ticket from the repository's configured tracker. Use when the user asks to start work, work on the next issue, pick up a ready ticket, or continue tracker-driven work.
---

# Work Next Ticket

Use this skill as the mandatory starting point before touching code for an issue-driven session.

The repository defines its own tracker and label vocabulary. Before acting, read:

- `AGENTS.md` at the repo root
- `docs/agents/issue-tracker.md`
- `docs/agents/triage-labels.md`

## Before Starting (Mandatory)

1. Run `./scripts/init.sh` and verify it finishes without errors.
   - If it fails, stop and fix the environment before touching application code.
2. Read the issue tracker using the workflow defined by the repository.
3. Choose exactly one task that is ready to implement according to the repository's triage mapping. Do not work on more than one issue at a time.

## Choosing the Task

1. List open work items using the repository's configured tracker workflow.
2. Filter to the repo's implementation-ready triage state — the canonical role is `ready-for-implementer`, but the actual tracker string may differ.
3. Choose the item with the lowest issue/ticket identifier unless the repository documents a different priority rule.
4. If the repository defines a "work started" label/status (for example `in-progress`), apply it before making code changes.
5. Document on the ticket that you are starting work and summarize the intended first step.

## Parent PR / MR Workflow

- Do not create a separate PR/MR for each child issue unless the repository explicitly requires it.
- If the repository uses parent/child planning, contribute child-issue commits to the parent review branch.
- If an existing parent PR/MR already exists, continue working on that branch.
- If no parent review artifact exists yet, create one only when the first child issue needs review and the repository workflow expects it.
- Child issue commits must use Conventional Commits format and should reference the child issue when useful.

## Tracker Writing Footer

When writing any issue/ticket comment, issue/ticket description, PR/MR description, or similar tracker text, always end the body with this italic footer:

```markdown

_Written by agent_
```

There must be exactly one blank line between the final sentence of the message and `_Written by agent_`.

## Hard Rules (Non-Negotiable)

- Work on only one child issue at a time. Do not mix changes from multiple child issues in the same commit.
- Do not declare a task done without green tests.
- Run `./scripts/init.sh` before finishing and ensure the test block passes 100%.
- Document progress on the ticket while you work, not only at the end.
- Leave the repository clean before closing the session.
- Do not create child-issue PRs/MRs unless the repository explicitly requires it.
- All tracker comments/descriptions written by the agent must include the `_Written by agent_` footer with one blank line before it.

## While Working

- Keep the ticket updated with meaningful progress notes, blockers, decisions, and test results.
- If docs exist for the relevant area, read them before implementing.
- Avoid broad refactors or unrelated cleanups unless they are required for the selected issue.
- Do not introduce temporary files, debug prints, or TODOs without context.

## If You Get Stuck

1. Re-read the relevant section in `docs/`, if present.
2. If a tool or project command does not behave as expected, do not invent a workaround.
3. Document the issue, command output, and current status on the ticket.
4. End the session cleanly instead of making speculative changes.

## Before Finishing

1. Run:
   ```bash
   ./scripts/init.sh
   ```
   Everything must be green.
2. If the task is finished, advance the ticket using the repository's tracker conventions:
   - Apply the `ready-for-reviewer` triage state for this repo.
   - Remove any `ready-for-implementer` or `in-progress` markers if the repo uses them.
3. Link the review artifact (PR/MR) to the child issue, if one exists or was created.
4. Confirm any commits created by the agent use Conventional Commits format and that the child issue's work is contained in a focused commit.
5. Ensure the repository is clean and contains no temporary files, debug prints, or unexplained TODOs.
6. Add a final ticket comment summarizing:
   - What changed
   - Tests run
   - PR/MR link, if applicable
   - Any follow-up needed
7. Ensure the final comment ends with the required footer:
   ```markdown

   _Written by agent_
   ```
