# Approval Gate

## Purpose

The approval gate defines when user approval is required before action execution.

The approval gate is mandatory for high-impact actions.

It prevents the agent or tools from silently performing destructive, external, or state-changing operations.

## Approval Requirement

Explicit approval is required for:

- delete
- deploy
- publish
- credentials
- permissions
- database migration
- external side effects
- Git state changes
- production changes
- server mutations
- public artifact creation
- automation triggers

## Git Operations Requiring Approval

The following Git operations require explicit approval:

- `git add`
- `git commit`
- `git push`
- `git pull`
- `git merge`
- `git rebase`
- `git checkout`
- `git reset`
- `git clean`
- branch creation
- branch deletion
- tag creation
- tag deletion

## External Side Effects Requiring Approval

The following require explicit approval:

- sending email
- publishing content
- deploying application
- calling external API with side effects
- modifying external service configuration
- creating public links
- triggering webhook
- triggering automation
- uploading files to external systems

## Credential and Permission Actions

Credential and permission actions are critical by default.

They must be blocked unless all conditions are true:

- user explicitly approved the exact action
- target is clear
- environment is clear
- allowed scope is narrow
- no credential value is exposed in logs or output
- rollback or recovery path exists when applicable

## Approval Scope

Approval must be scoped.

Bad approval:

    Do whatever is needed.

Good approval:

    Create the five Markdown contract files under policy-engine only. Do not commit or push.

Good approval:

    Run git add for policy-engine Markdown files only. Do not commit.

Good approval:

    Commit staged policy-engine files with message: Add policy engine contract. Do not push.

## Approval Expiration

Approval applies only to the current requested action.

Approval does not automatically carry over to:

- commit
- push
- deploy
- delete
- production change
- permission change
- database change
- external side effect

Each high-impact action requires a separate approval.

## Rejected Approval

If the user rejects approval, the decision must be:

    {
      "decision": "block",
      "risk_level": "high",
      "reason": "User rejected approval for the requested action.",
      "required_approval": false,
      "allowed_scope": "No execution allowed."
    }

## Missing Approval

If approval is missing for a high-impact action, the decision must be:

    {
      "decision": "require_approval",
      "risk_level": "high",
      "reason": "Requested action changes system state and requires explicit approval.",
      "required_approval": true,
      "allowed_scope": "No execution until approval is granted."
    }

## Read-Only Exception

Read-only inspection may proceed without additional approval when:

- target is clear
- environment is clear
- no state changes occur
- no credentials are exposed
- no external side effects occur

Examples:

- `git status`
- `git log --oneline`
- `find policy-engine -type f`
- `cat README.md`
