# ICL Project — Copilot Instructions

You are working on the **Intent Contract Language (ICL)** project — a formal, deterministic, language-agnostic specification language for intent contracts.

## Project Structure

- **ICL-Spec** — The standard (BNF grammar, specification, conformance tests). ZERO code.
- **ICL-Runtime** — Canonical Rust implementation (Cargo workspace: `icl-core` lib + `icl-cli` bin)
- **ICL-Docs** — Documentation website (mdBook)

## Core Principles

1. **Determinism is non-negotiable** — same input → identical output, always
2. **Spec is authoritative** — code must match `CORE-SPECIFICATION.md`
3. **Single Rust codebase** — compiled to all targets (native, Python/PyO3, JS/WASM, Go/cgo)
4. **Honesty** — docs reflect actual state, never aspirations

## Current Phase

Phase 10 Complete — All phases through Multi-Target JS Binding complete, v0.1.4 published to crates.io/PyPI/npm. See `ICL-Runtime/ROADMAP.md` for details.

## Key Files

- `ICL-Runtime/ROADMAP.md` — progress tracker (check boxes as you complete items)
- `ICL-Docs/PLAN.md` — architecture decisions
- `ICL-Spec/grammar/icl.bnf` — formal grammar
- `crates/icl-core/src/parser/` — parser implementation

## Git

- SSH alias: `github-assetexpand2` (not `github.com`)
- After every x.x sub-phase: `git commit && git push`
- Remotes: `git@github-assetexpand2:ICL-System/<repo>.git`

## Rust Environment

- Rust 1.93.0 at `$HOME/.cargo/bin`
- Always `source "$HOME/.cargo/env"` before cargo commands
