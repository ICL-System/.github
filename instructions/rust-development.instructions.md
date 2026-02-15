---
applyTo: "**/*.rs"
---

# Rust Development — ICL Standards

## Determinism Rules (Non-Negotiable)

- Use `BTreeMap`/`BTreeSet` — NEVER `HashMap`/`HashSet`
- No `rand`, no randomness, no system time in `icl-core`
- IEEE 754 floating point — specify rounding mode
- No `unwrap()` in library code — use `?` or `.ok_or(Error::...)?`
- All iteration order must be deterministic

## Error Handling

- All fallible operations return `crate::Result<T>`
- Use `thiserror` for error types
- Error messages include line:column when from parser
- Never swallow errors

## Naming

- Types: `PascalCase` (e.g., `ContractNode`)
- Functions: `snake_case` (e.g., `parse_contract`)
- Constants: `SCREAMING_SNAKE_CASE`

## Testing

- Every public function has a unit test
- Every error path has a test
- 100-iteration determinism proof: same input → identical output × 100
- Idempotence proof where applicable: `f(f(x)) == f(x)`

## Architecture

```
ICL Text → Parser → AST → Normalizer → Canonical Form
                           ↓
                        Verifier → Type Check + Invariants
                           ↓
                        Executor → Sandboxed Execution
```

## Reference

- Spec: `ICL-Spec/spec/CORE-SPECIFICATION.md`
- Grammar: `ICL-Spec/grammar/icl.bnf`
- Error types: `crates/icl-core/src/error.rs`
- Public API: `crates/icl-core/src/lib.rs`
