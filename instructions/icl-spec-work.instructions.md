---
applyTo: "**/ICL-Spec/**,**/*.icl"
---

# ICL Spec Work

The specification is the authoritative reference. `ICL-Spec/` has ZERO code.

## Core Specification Rules

- ✅ Can clarify documentation
- ✅ Can add explanations and examples
- ❌ Cannot change grammar
- ❌ Cannot add new primitives
- ❌ Cannot change semantics

## ICL Types

**Primitives:** Integer, Float, String, Boolean, ISO8601, UUID
**Composites:** Object { fields }, Enum ["a", "b"]
**Collections:** Array<T>, Map<K, V>

## Contract Sections (all required except Extensions)

1. Identity — stable_id, version, created_timestamp, owner, semantic_hash
2. PurposeStatement — narrative, intent_source, confidence_level
3. DataSemantics — state (typed fields), invariants (string expressions)
4. BehavioralSemantics — operations (name, pre/postcondition, parameters, side_effects, idempotence)
5. ExecutionConstraints — trigger_types, resource_limits, external_permissions, sandbox_mode
6. HumanMachineContract — system_commitments, system_refusals, user_obligations
7. Extensions (optional) — system-specific extension blocks
