---
applyTo: "**/*test*,**/tests/**,**/conformance/**,**/determinism/**"
---

# Testing & Determinism — ICL

## Every Test Must Prove

1. **Correctness** — output matches spec
2. **Determinism** — 100 iterations produce identical output
3. **Idempotence** (where applicable) — `f(f(x)) == f(x)`

## Test Structure

```rust
#[test]
fn test_feature_works() {
    // Test the happy path
}

#[test]
fn test_feature_error_case() {
    // Test specific error conditions
}

#[test]
fn test_feature_determinism_100_iterations() {
    let input = ...;
    let first = process(&input).unwrap();
    for i in 0..100 {
        let result = process(&input).unwrap();
        assert_eq!(first, result, "Determinism failure at iteration {}", i);
    }
}
```

## Conformance Testing

- `ICL-Spec/conformance/valid/` — parser MUST accept all of these
- `ICL-Spec/conformance/invalid/` — parser MUST reject all with correct error
- `ICL-Spec/conformance/normalization/` — normalizer must produce expected output

## Rules

- No `#[ignore]` without a tracking issue
- No `unwrap()` in test setup — use `.expect("description")`
- Test file names match module names: `tokenizer.rs` tests in `tokenizer::tests`
- Integration tests go in `tests/` directory
