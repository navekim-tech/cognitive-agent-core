# Decision Contract

## Purpose

This file defines the required output contract for every policy-engine decision.

Every action request must receive exactly one policy decision before execution.

## Decision Object

A policy decision must include the following fields:

    {
      "decision": "allow",
      "risk_level": "low",
      "reason": "Read-only repository inspection with clear scope.",
      "required_approval": false,
      "allowed_scope": "Inspect repository status only.",
      "audit_event": {
        "event_type": "policy_decision",
        "action_type": "inspect",
        "decision": "allow",
        "risk_level": "low"
      }
    }

## Required Fields

### decision

Allowed values:

- `allow`
- `require_approval`
- `block`
- `ask_clarification`

### risk_level

Allowed values:

- `low`
- `medium`
- `high`
- `critical`

### reason

A short English explanation for the decision.

The reason must be operational, not conversational.

Good example:

    Git push modifies remote repository state and requires explicit approval.

Bad example:

    This might be risky, so better ask first.

### required_approval

Boolean.

Allowed values:

- `true`
- `false`

### allowed_scope

A precise boundary for what may happen.

Examples:

- `Read files under policy-engine only.`
- `Create Markdown files only under policy-engine root.`
- `Run git status and git log only.`
- `No network calls.`
- `No production changes.`

### audit_event

A structured event intended for `memory-events`.

The audit event records the policy decision and the reason for it.

## Decision Semantics

### allow

The action may proceed within the declared `allowed_scope`.

Use only when:

- target is clear
- environment is clear
- risk is low
- no external side effect exists
- no destructive operation exists
- approval is not required

### require_approval

The action must not execute until explicit user approval is received.

Use when:

- operation modifies files
- operation changes Git state
- operation affects external systems
- operation may expose secrets
- operation affects permissions
- operation affects databases
- operation deploys or publishes
- confidence is insufficient for safe execution

### block

The action must not execute.

Use when:

- user rejected approval
- action is unsafe even with approval
- target would expose credentials
- action attempts destructive production behavior without safe rollback
- source is untrusted
- requested scope is broader than allowed
- policy cannot create a safe bounded action

### ask_clarification

The action must pause and request missing information.

Use when:

- target is unclear
- environment is unclear
- approval state is unclear
- action type is ambiguous
- allowed scope cannot be determined
- confidence is too low to classify the action safely

## Approval State Handling

### user_approval: none

No approval exists.

High-impact actions must return `require_approval`.

### user_approval: requested

Approval has been requested but not granted.

The action must not execute.

Decision must remain `require_approval`.

### user_approval: approved

The action may proceed only within the approved scope.

If the requested action exceeds approved scope, return `require_approval` again.

### user_approval: rejected

The action must return `block`.

## Confidence Handling

Low confidence must never produce `allow` for write, external, destructive, credential, permission, database, deployment, or Git state-changing actions.

If confidence is low and the action is low-risk read-only, return `ask_clarification` unless the scope is clear.
