---
applyTo: "**"
---

# Git Workflow — ICL

## SSH Remote

All repos use SSH host alias: `git@github-assetexpand2:ICL-System/<repo>.git`

## Commit Message Format

```
type(scope): Brief description

- What changed
- Why it changed
```

Types: `feat`, `fix`, `refactor`, `chore`, `docs`, `test`
Scopes: `parser`, `normalizer`, `verifier`, `executor`, `cli`, `spec`, `docs`

## Rules

1. **ALWAYS `cd` into the repo directory before any git command** — the workspace root is NOT a git repo. Use `cd <ICL_WORKSPACE>/ICL-Runtime` (or `ICL-Spec`, `ICL-Docs`).
2. `cargo build && cargo test` must pass before every commit
3. After finishing each x.x sub-phase: always `git commit && git push` (from inside the repo directory)
4. Commit messages reference the phase: "Phase 1.1", "Phase 1.2", etc.
5. Never force-push to main

## 3 Repos

| Repo | Remote |
|------|--------|
| ICL-Spec | `git@github-assetexpand2:ICL-System/ICL-Spec.git` |
| ICL-Runtime | `git@github-assetexpand2:ICL-System/ICL-Runtime.git` |
| ICL-Docs | `git@github-assetexpand2:ICL-System/ICL-Docs.git` |
| .github | `git@github-assetexpand2:ICL-System/.github.git` |
