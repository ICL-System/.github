---
applyTo: "**/parser/**"
---

# Parser Development — ICL

The parser is Phase 1 and the foundation. It converts ICL text → AST matching the BNF grammar exactly.

## Pipeline

`ICL text → Tokenizer → Token stream → Parser → AST (ContractNode)`

## Reference Files

- `ICL-Spec/spec/CORE-SPECIFICATION.md` Section 1 — BNF grammar (authoritative)
- `ICL-Spec/grammar/icl.bnf` — standalone grammar
- `ICL-Spec/conformance/valid/` — must parse successfully
- `ICL-Spec/conformance/invalid/` — must fail with correct errors
- `crates/icl-core/src/parser/tokenizer.rs` — tokenizer (done)
- `crates/icl-core/src/parser/ast.rs` — AST types (done)
- `crates/icl-core/src/parser/mod.rs` — parser entry point

## Checklist

1. Read the BNF production rule you're implementing
2. Write the AST type first (what the parser produces)
3. Write the parser function (recursive descent)
4. Write unit test against conformance fixtures
5. Error messages must include line:column
6. 100-iteration determinism test
7. Test against both valid/ and invalid/ conformance fixtures

## Rules

- Parser is pure: no side effects, no I/O, no randomness
- All state is in the token stream position
- Error recovery: report multiple errors, don't stop at first
- AST nodes carry Span for error reporting
