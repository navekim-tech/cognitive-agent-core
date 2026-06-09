# Policy Audit Events

## Purpose

This file defines how `policy-engine` decisions are recorded as audit events inside `memory-events`.

It extends `event_schema.md`.

It does not replace the core memory event schema.

## Principle

Every policy decision may produce an audit event.

Not every audit event becomes long-term memory.

A policy audit event becomes memory only when it changes future caution, verification depth, escalation sensitivity, workflow preference, or trust weighting.

## Policy Decisions to Audit

The following decisions should be auditable:

- `allow`
- `require_approval`
- `block`
- `ask_clarification`

## Required Audit Fields

Each policy audit event should include:

- `event_id`
- `timestamp`
- `event_type`
- `policy_decision`
- `action_type`
- `target`
- `environment`
- `risk_level`
- `reason`
- `required_approval`
- `allowed_scope`
- `user_approval`
- `confidence`
- `source`
- `lesson`
- `behavioral_effect`
- `persistence`
- `decay_policy`

## Event Type Mapping

### allow

Policy decision:

    allow

Recommended event type:

    success

Default severity:

    0.1 - 0.3

Behavioral effect:

    reinforce safe low-risk execution patterns

Persistence:

    temporary or session

### require_approval

Policy decision:

    require_approval

Recommended event type:

    approval

Default severity:

    0.4 - 0.7

Behavioral effect:

    reinforce approval gate usage and escalation sensitivity

Persistence:

    session or long_term

### block

Policy decision:

    block

Recommended event type:

    warning or rejection

Default severity:

    0.7 - 1.0

Behavioral effect:

    increase caution and future verification depth

Persistence:

    long_term or permanent

### ask_clarification

Policy decision:

    ask_clarification

Recommended event type:

    warning

Default severity:

    0.3 - 0.6

Behavioral effect:

    increase clarification before uncertain execution

Persistence:

    temporary or session

## Audit Event Example

    {
      "event_id": "evt_policy_0001",
      "timestamp": "ISO-8601 timestamp",
      "event_type": "approval",
      "policy_decision": "require_approval",
      "action_type": "git_push",
      "target": "origin main",
      "environment": "repo",
      "risk_level": "high",
      "reason": "Git push modifies remote repository state and requires explicit approval.",
      "required_approval": true,
      "allowed_scope": "Push approved commit to origin main only.",
      "user_approval": "approved",
      "confidence": 0.98,
      "source": "user",
      "lesson": "Git remote mutations require explicit scoped approval.",
      "behavioral_effect": "increase approval sensitivity for Git remote operations",
      "persistence": "long_term",
      "decay_policy": "manual_review"
    }

## Memory Boundary

Policy audit events must not directly modify core rules.

They may influence:

- caution level
- workflow preference
- trust weighting
- verification depth
- escalation sensitivity

They must not modify:

- operational constitution
- human authority
- core safety boundaries
- approval requirement for high-impact actions

## Retention Rule

Do not persist every low-risk `allow` event.

Persist only events that are useful for future behavior.

Examples worth persisting:

- repeated approval requirements for the same workflow
- blocked unsafe actions
- rejected approvals
- unclear targets that caused repeated clarification
- high-risk operations that required explicit approval
- production or credential-related policy decisions

Examples usually not worth persisting:

- routine read-only inspection
- successful low-risk file reads
- ordinary repository listing
- repeated safe `git status` checks

## Enforcement Note

The policy engine decides whether an action may proceed.

The memory event records why that decision happened.

Policy must remain upstream of memory.

Memory must never override policy.
