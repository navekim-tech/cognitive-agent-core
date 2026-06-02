# Risk Matrix

## Purpose

This file defines the initial risk model for the policy engine.

Risk is determined by action impact, target sensitivity, environment, reversibility, and external side effects.

## Risk Levels

### low

Low-risk actions do not modify system state.

Examples:

- read file
- list directory
- inspect Git status
- inspect Git log
- search repository content
- read documentation

Default decision: `allow`, if scope is clear.

### medium

Medium-risk actions modify local or repository content but are bounded and reversible.

Examples:

- create Markdown file
- edit documentation
- create local directory
- update non-sensitive configuration
- stage files with Git

Default decision: `require_approval` unless explicitly pre-approved.

### high

High-risk actions affect repository state, remote state, deployment behavior, data, or external systems.

Examples:

- Git commit
- Git push
- Git pull
- Git merge
- Git checkout
- Git rebase
- deploy
- publish
- call external API with side effects
- run database migration
- change runtime configuration

Default decision: `require_approval`.

### critical

Critical-risk actions may expose secrets, damage production, destroy data, or change access control.

Examples:

- delete production data
- drop database table
- expose credentials
- modify permissions
- rotate secrets
- reset repository state
- delete branches
- remove authentication controls
- destructive production deploy

Default decision: `block` unless a narrowly scoped explicit approval exists and safe rollback is available.

## Risk Escalation Rules

Risk must escalate to at least `medium` if:

- action writes to repository
- action creates or modifies files
- action changes configuration
- action uses tool output to modify state

Risk must escalate to at least `high` if:

- action changes Git state
- action affects remote repository
- action affects server state
- action affects deployment
- action has external side effects
- action touches database schema or records
- action modifies automation behavior

Risk must escalate to `critical` if:

- action touches credentials
- action changes permissions
- action affects production data destructively
- action may expose private data
- action has irreversible impact
- action has unclear target in production
- approval was rejected but execution is still attempted

## Environment Risk

### local

Base environment risk: `low`

Escalate based on action type.

### repo

Base environment risk: `medium`

Any Git state-changing action requires approval.

### server

Base environment risk: `high`

Any write, deploy, permission, database, or network side-effect requires approval.

### production

Base environment risk: `critical`

No production mutation may run without explicit approval and bounded scope.

### external

Base environment risk: `high`

Any external side effect requires approval.

## Reversibility

Actions that are hard to reverse must increase risk.

Examples:

- deletion
- reset
- overwrite
- database migration
- permission change
- production deploy
- public publishing

## Default Safe Behavior

When risk cannot be determined, the policy engine must not allow the action.

Use:

- `ask_clarification` if missing information can make the action safe
- `require_approval` if risk is known but approval is missing
- `block` if the requested action cannot be made safe
