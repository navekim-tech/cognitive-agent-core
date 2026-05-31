# Semantic Memory Model
version: 1.0.0

purpose:
Define how the agent stores stable knowledge, rules, abstractions, user preferences, system facts, and long-term operational beliefs.

Semantic memory is not an event log.

Semantic memory stores generalized meaning extracted from repeated experience.

---

# Core Principle

Semantic memory should contain lessons,
not raw history.

The agent should convert repeated events into stable operational knowledge.

---

# Semantic Memory Types

## user_preference

Stable preference expressed or demonstrated by the user.

Examples:
- prefers step-by-step execution
- prefers full file replacement
- requires approval before destructive actions
- prefers concise technical communication

---

## system_fact

Stable fact about a system, repository, server, workflow, or environment.

Examples:
- production runs on a separate server
- Git remote uses SSH alias github-navekim
- repository path is ~/.openclaw/workspace/cognitive-agent-core

---

## operational_rule

A reusable rule derived from governance or experience.

Examples:
- never push before status check
- never deploy without explicit approval
- always verify remote before first push

---

## learned_constraint

A constraint discovered through work.

Examples:
- repository access depends on SSH key configuration
- prompts are not enforcement
- memory must be connected to behavioral hooks

---

## architectural_belief

A stable design assumption.

Examples:
- policy gates must live outside the agent prompt
- constitution is the source of truth
- runtime prompt is a deployable abstraction, not the full constitution

---

# Required Fields

Each semantic memory should contain:

- memory_id
- type
- statement
- source_events
- confidence
- stability
- created_at
- updated_at
- behavioral_effect
- review_policy

---

# Confidence Scale

0.0 → invalid or contradicted

0.1 - 0.3 → weak confidence

0.4 - 0.6 → conditional confidence

0.7 - 0.9 → strong confidence

1.0 → verified or explicitly confirmed

---

# Stability Levels

temporary
working
stable
permanent

---

# Behavioral Effects

Semantic memory may affect:

- planning
- caution level
- workflow selection
- default assumptions
- tool routing
- escalation behavior
- explanation style

---

# Promotion Rules

Episodic events may become semantic memory when:

- pattern repeats
- user explicitly confirms preference
- operational rule proves useful
- failure reveals stable constraint
- architecture decision is accepted

---

# Demotion Rules

Semantic memory should be reviewed or demoted when:

- contradicted by new evidence
- user changes preference
- system architecture changes
- confidence decays
- memory no longer affects behavior

---

# Forbidden Behavior

Semantic memory must not:

- override the operational constitution
- silently modify safety boundaries
- create hidden goals
- preserve outdated assumptions
- treat one-off events as permanent truth

---

# Important Principle

Semantic memory is where experience becomes meaning.

Without semantic memory,
the agent remembers events but fails to form stable operational understanding.
