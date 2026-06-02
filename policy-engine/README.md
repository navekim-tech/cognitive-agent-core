# Policy Engine

## Purpose

The `policy-engine` is the enforcement layer between an intended action and its execution.

It is not a prompt.
It is not memory.
It is not a recommendation layer.

It returns a binding operational decision that determines whether an action may proceed, must wait for approval, must be blocked, or requires clarification.

## Core Responsibility

The policy engine evaluates an action request before execution and produces a structured decision.

It protects the system from unsafe, unclear, destructive, external, or high-impact operations.

## First-Stage Architecture

At this stage, the policy engine is defined as a Markdown contract.

This is intentional.

The goal is to stabilize the operational rules before converting them into JSON Schema, Python, TypeScript, or runtime enforcement code.

## Inputs

The policy engine receives:

- `action_type`
- `target`
- `environment`
- `risk_signals`
- `user_approval`
- `confidence`
- `source`

## Outputs

The policy engine returns:

- `decision`
- `risk_level`
- `reason`
- `required_approval`
- `allowed_scope`
- `audit_event`

## Decision Types

Allowed decision values:

- `allow`
- `require_approval`
- `block`
- `ask_clarification`

## Risk Levels

Allowed risk levels:

- `low`
- `medium`
- `high`
- `critical`

## Initial Enforcement Rules

High-impact operations must not execute silently.

The following action categories require explicit user approval or must be blocked if approval is absent:

- delete
- deploy
- publish
- credentials
- permissions
- database migration
- external side effects
- Git write operations

Unclear target or unclear environment must return `ask_clarification`.

Read-only inspection may be allowed when scope is clear and user intent is confirmed.

## Current Status

This module is a contract-only layer.

No runtime implementation exists yet.
