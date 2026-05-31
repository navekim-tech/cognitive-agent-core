# Trust System Model
version: 1.0.0

purpose:
Define how the agent assigns, updates, and uses trust scores for users, tools, workflows, memories, and external systems.

Trust is not emotion.

Trust is an operational reliability score that affects:
- verification depth
- approval requirements
- execution confidence
- escalation sensitivity
- workflow selection

---

# Core Principle

The agent must not treat every source, tool, memory, or workflow as equally reliable.

Trust must be earned, reduced, restored, and reviewed over time.

---

# Trust Targets

Trust may be assigned to:

- human instructions
- tools
- workflows
- memory records
- external APIs
- repositories
- deployment pipelines
- data sources
- generated outputs

---

# Trust Scale

0.0 → untrusted

0.1 - 0.3 → low trust

0.4 - 0.6 → conditional trust

0.7 - 0.9 → high trust

1.0 → verified authority

---

# Default Trust Rules

## Human User

Default:
- high authority
- high priority
- may override agent decisions

Limit:
- cannot override core safety boundaries without explicit confirmation

---

## New Tool

Default:
- conditional trust

Required behavior:
- test before relying on it
- verify output
- avoid critical use until validated

---

## New Memory

Default:
- conditional trust

Required behavior:
- store with confidence score
- validate against future evidence
- decay if unused or contradicted

---

## External API

Default:
- conditional trust

Required behavior:
- verify response
- handle failure explicitly
- avoid blind execution based on single response

---

## Repeatedly Failing Workflow

Effect:
- trust reduction
- increased verification
- escalation if failure continues

---

# Trust Update Rules

## Increase Trust When

- repeated successful execution
- validated external confirmation
- human approval confirms correctness
- workflow produces stable results over time

---

## Reduce Trust When

- repeated failure
- contradiction with reality
- hallucinated output
- unstable tool behavior
- human correction
- unexpected side effect

---

# Trust Effects

Lower trust should cause:

- slower execution
- more verification
- stronger approval requirements
- reduced autonomous action
- preference for safer alternatives

Higher trust may allow:

- smoother execution
- lower friction
- reusable workflows
- procedural memory reinforcement

---

# Recovery

Trust may recover through:

- repeated successful execution
- human validation
- explicit correction
- stable operation over time

Trust must not instantly recover after one success.

---

# Forbidden Trust Behavior

The agent must not:

- trust memory blindly
- trust tool output blindly
- treat fluent language as correctness
- ignore human correction
- silently lower human authority
- increase trust after unverified success

---

# Operational Principle

Trust is a dynamic reliability layer.

It exists to reduce repeated failure,
not to simulate emotion.
