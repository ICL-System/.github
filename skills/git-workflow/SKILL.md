---
name: git-workflow
description: Use when committing, branching, creating PRs, or managing Git operations across the 3 ICL repos. Covers commit message format, branch naming, cross-repo coordination, and SSH remote configuration.
---

# Git Workflow

## When to Use

- Making a commit
- Creating a branch
- Pushing to remote
- Managing multiple repos (ICL-Spec, ICL-Runtime, ICL-Docs)

## Context

ICL has 3 repos under one GitHub org. Commits must be clean, messages must be clear, and cross-repo changes must be coordinated. All repos use SSH host alias `github-assetexpand2`.

Reference files:
- `CONTRIBUTING.md` — commit message format
- `ICL-Runtime/ROADMAP.md` — current phase determines what branches exist

## SSH Remotes

| Repo | Remote |
|------|--------|
| ICL-Spec | `git@github-assetexpand2:ICL-System/ICL-Spec.git` |
| ICL-Runtime | `git@github-assetexpand2:ICL-System/ICL-Runtime.git` |
| ICL-Docs | `git@github-assetexpand2:ICL-System/ICL-Docs.git` |

## Commit Message Format

```
type(scope): Brief description

- What changed
- Why it changed
```

Types: `feat`, `fix`, `refactor`, `chore`, `docs`, `test`
Scopes: `parser`, `normalizer`, `verifier`, `executor`, `cli`, `spec`, `docs`

### Component Tags (alternative format)

| Tag | Scope |
|-----|-------|
| `[parser]` | Tokenizer, parser, AST |
| `[normalizer]` | Canonical normalizer |
| `[verifier]` | Type checker, invariant verifier, determinism checker |
| `[executor]` | Execution engine |
| `[cli]` | CLI tool |
| `[spec]` | Specification changes (ICL-Spec repo) |
| `[docs]` | Documentation changes |
| `[ci]` | CI/CD pipeline |
| `[meta]` | Project structure, config, tooling |

## Procedure

1. **ALWAYS `cd` into the specific repo directory first** — the ICL workspace root is NOT a git repo
   ```bash
   cd <ICL_WORKSPACE>/ICL-Runtime   # or ICL-Spec, ICL-Docs
   ```
2. Ensure you're on the correct branch
3. Stage only related changes (`git add -p` for partial staging)
4. Write commit message following the format above
5. Verify build passes before committing (`source "$HOME/.cargo/env" && cargo build && cargo test`)
6. Push to remote
7. If changes span repos: `cd` into each repo and commit separately with cross-references

## Branch Naming

```
main                    # Stable, always builds
dev                     # Integration branch (optional)
feature/<short-desc>    # Feature work
fix/<short-desc>        # Bug fixes
phase-0/restructure     # Phase-level branches (for major work)
```

## Rules

- **Never force-push to main**
- **Every commit builds** — no broken commits on any branch
- **Atomic commits** — one logical change per commit
- **No `WIP` or `temp` commits on main** — squash before merging
- **Cross-repo changes reference each other** — "See ICL-Spec commit abc123"
- **Tag releases** — `v0.1.0` format, same version across all repos
- After finishing each x.x sub-phase: always `git commit && git push`

## Anti-Patterns

- Giant commits with 20+ files ("restructure everything")
- Vague messages ("fix stuff", "update", "wip")
- Committing generated files (target/, node_modules/, __pycache__/)
- Different versions across repos
- Committing secrets, keys, or credentials
