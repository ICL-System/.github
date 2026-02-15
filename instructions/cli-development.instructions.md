---
applyTo: "**/icl-cli/**,**/main.rs"
---

# CLI Development — ICL

The CLI lives in `crates/icl-cli/` and is a thin wrapper over `icl-core`.

## Commands

```bash
icl validate <file.icl>     # Check syntax + types
icl normalize <file.icl>    # Output canonical form
icl verify <file.icl>       # Full verification
icl fmt <file.icl>          # Format to standard style
icl hash <file.icl>         # Compute semantic hash (SHA-256)
icl diff <a.icl> <b.icl>    # Semantic diff
icl init [name]             # Scaffold new contract
icl execute <file.icl>      # Run contract
icl version                 # Show version
```

## Rules

- ZERO logic in CLI — all logic in `icl-core`
- Exit codes: 0 = success, 1 = validation failure, 2 = runtime error
- Errors to stderr, output to stdout
- Support `--json` for machine-readable output
