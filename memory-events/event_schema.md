# Memory Event Schema
version: 1.0.0

purpose:
Define how operational events are represented,
classified, weighted, and allowed to influence future behavior.

An event is not memory by itself.

Memory exists only when an event modifies future cognition or execution behavior.

---

# Event Structure

Every event should contain:

- event_id
- timestamp
- event_type
- context
- severity
- confidence
- source
- lesson
- behavioral_effect
- persistence
- decay_policy

---

# Core Event Types

## failure

Operational failure.

Examples:
- deployment failure
- broken migration
- invalid API call
- permission issue

Behavioral impact:
- increase caution
- increase verification
- reduce trust in unstable workflows

---

## success

Successful execution with validated outcome.

Behavioral impact:
- reinforce workflow confidence
- strengthen procedural memory

---

## warning

Potential instability or elevated uncertainty.

Behavioral impact:
- temporary caution increase
- request additional validation

---

## approval

Explicit human authorization.

Behavioral impact:
- allow gated execution
- reinforce governance pathways

---

## rejection

Human denial or blocked action.

Behavioral impact:
- reduce confidence in proposed strategy
- strengthen future approval sensitivity

---

## preference

Stable human preference.

Examples:
- prefers step-by-step execution
- prefers full file replacement
- avoid partial patches

Behavioral impact:
- persistent workflow adaptation

---

# Severity Scale

0.0 → informational only

0.1 - 0.3 → low operational importance

0.4 - 0.6 → moderate behavioral relevance

0.7 - 0.9 → major behavioral impact

1.0 → critical system-level event

---

# Persistence Types

temporary
session
long_term
permanent

---

# Decay Policies

none
time_decay
usage_decay
confidence_decay
manual_review

---

# Behavioral Mutation Principle

Events are allowed to modify:

- caution level
- workflow preference
- trust weighting
- verification depth
- escalation sensitivity

Events are NOT allowed to modify:

- operational constitution
- human authority
- core safety boundaries

---

# Important Principle

The system should not remember everything.

The system should remember:
- repeated patterns
- failures
- meaningful successes
- stable preferences
- operational risks
- governance signals

Memory quality is more important than memory quantity.
