# ICL — Intent Contract Language

[![crates.io](https://img.shields.io/crates/v/icl-runtime.svg)](https://crates.io/crates/icl-runtime)
[![npm](https://img.shields.io/npm/v/icl-runtime.svg)](https://www.npmjs.com/package/icl-runtime)
[![PyPI](https://img.shields.io/pypi/v/icl-runtime.svg)](https://pypi.org/project/icl-runtime/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://github.com/ICL-System/ICL-Runtime/blob/main/LICENSE)

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


## How is ICL Different from Guardrails?

Guardrails and system prompts are *suggestions* an LLM can ignore or misinterpret. ICL contracts are **mathematically enforced walls** — verified before execution, impossible to bypass.

| | System Prompts | Guardrails | **ICL** |
|---|---|---|---|
| **What it is** | Natural language instructions | Runtime filters | Formal, verified contracts |
| **Enforcement** | LLM interprets (may ignore) | Probabilistic | **Mathematical proof** |
| **Analogy** | "Please don't" | Smoke detector | **Fireproof wall** |

**Where ICL is used:** Trading agents that CANNOT exceed limits · Surgical robots that MUST stop if sensors fail · Drones that CANNOT enter restricted airspace · Code deploy agents that CANNOT ship without passing tests

> See the full [ICL vs Guardrails — 50+ real-world examples](https://icl-system.github.io/ICL-Docs/use-cases.html) for when (and when not) to use ICL.



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
