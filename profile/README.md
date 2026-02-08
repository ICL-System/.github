# ICL — Intent Contract Language

> A formal, deterministic, language-agnostic specification language for intent contracts.

## What is ICL?

ICL defines a standard way to express **intent contracts** — machine-verifiable declarations of what a system intends to do. Every contract is deterministic (same input → identical output), verifiable (all properties machine-checkable), and bounded (execution bounded in memory and time).

```
contract HelloWorld {
  version: "1.0"
  identity {
    name: "hello-world"
  }
  intent {
    action: "greet"
    description: "A simple greeting contract"
  }
}
```

## Repositories

| Repository | Description |
|------------|-------------|
| **[ICL-Spec](https://github.com/ICL-System/ICL-Spec)** | The standard — BNF grammar, specification, conformance tests |
| **[ICL-Runtime](https://github.com/ICL-System/ICL-Runtime)** | Canonical Rust implementation (native, Python, JS/WASM, Go) |
| **[ICL-Docs](https://github.com/ICL-System/ICL-Docs)** | Documentation website |

## Install

```bash
# Rust
cargo install icl-cli

# Python
pip install icl-runtime

# JavaScript
npm install icl-runtime
```

## Architecture

One Rust codebase, compiled to every target:

```
ICL Text → Parser → AST → Normalizer → Canonical Form
                           ↓
                        Verifier → Type Check + Invariants + Determinism
                           ↓
                        Executor → Sandboxed Execution
```

## Core Principles

- **Determinism is non-negotiable** — same input → identical output, always
- **Spec is authoritative** — the specification defines truth, code must match
- **Single codebase** — all logic in Rust, compiled to every platform
- **Verifiable** — all contract properties are machine-checkable
- **Bounded** — all execution bounded in memory and time

## License

MIT
