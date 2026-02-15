---
name: debugging
description: Use when investigating bugs, test failures, unexpected behavior, or compile errors in any ICL component. Provides systematic debugging procedures for the parser, normalizer, verifier, and executor pipeline.
---

# Debugging

## When to Use

- A test fails unexpectedly
- `cargo build` or `cargo test` produces errors
- A binding produces different results than Rust core
- Behavior doesn't match the spec
- User reports a bug

## Context

ICL's determinism guarantee means bugs are especially serious — non-determinism anywhere breaks the core promise. Debugging must be systematic, not guess-and-check.

Reference files:
- `CORE-SPECIFICATION.md` — is the behavior spec-compliant?
- `crates/icl-core/src/error.rs` — error types
- Test files in the failing module

## Procedure

1. **Reproduce** — Write the minimal input that triggers the bug
2. **Read the error** — Full error message, stack trace, line numbers
3. **Locate** — Which module? parser → normalizer → verifier → executor?
4. **Spec check** — Read the relevant spec section. Is the code wrong or the expectation wrong?
5. **Isolate** — Write a failing unit test that captures exactly the bug
6. **Diagnose** — Use binary search if unsure:
   - Does the parser produce correct AST? → If no, parser bug
   - Does the normalizer produce correct canonical form? → If no, normalizer bug
   - Does the verifier report correctly? → If no, verifier bug
   - Does the executor produce correct output? → If no, executor bug
7. **Fix** — Make the minimal change that fixes the failing test
8. **Verify** — Run the full test suite, not just the fixed test
9. **Determinism** — Run 100-iteration determinism test on the fixed code
10. **Document** — Add a comment explaining WHY the bug existed and how the fix works

## Rules

- **Never fix without a failing test first** — write the test, then fix
- **Minimal fix only** — don't refactor while debugging
- **Check spec before assuming code is wrong** — maybe the test expectation is wrong
- **Determinism check after every fix** — a fix that breaks determinism is not a fix
- **No temporary workarounds without tracking issue** — `// HACK:` requires an issue link

## Debugging Pipeline

```
Bug Report / Failure
       ↓
Reproduce (minimal input)
       ↓
Isolate (which pipeline stage?)
       ↓
    ┌──────────────────────────────────┐
    │ Parser → Normalizer → Verifier → Executor │
    │   ↓          ↓           ↓          ↓      │
    │ Wrong AST  Wrong form  Wrong check  Wrong output│
    └──────────────────────────────────┘
       ↓
Write failing test
       ↓
Minimal fix
       ↓
Full test suite + determinism proof
```

## Common ICL Bug Categories

| Symptom | Likely cause |
|---------|-------------|
| Parse error on valid input | Tokenizer missing a token type or keyword |
| Different output on same input | Non-deterministic HashMap iteration (use BTreeMap) |
| `normalize(normalize(x)) != normalize(x)` | Normalizer not handling some edge case |
| Type error on valid contract | Type checker missing a composite/collection type |
| Binding returns different result | Type conversion error in FFI layer |

## Anti-Patterns

- Guessing and changing random things until tests pass
- Fixing the test assertion instead of the code
- Refactoring during debugging (separate concerns)
- "Works on my machine" — always reproduce in CI-equivalent environment
- Ignoring intermittent failures (ICL must be 100% deterministic)
