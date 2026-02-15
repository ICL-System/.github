# .github — ICL-System Organization

Organization-level GitHub configuration and AI-assisted development infrastructure for the [ICL-System](https://github.com/ICL-System) organization.

## Repository Structure

```
.github/
├── profile/
│   └── README.md            # GitHub org profile
├── copilot-instructions.md  # Top-level Copilot context
├── instructions/            # Coding guidelines (8 files)
│   ├── cli-development.instructions.md
│   ├── documentation.instructions.md
│   ├── git-workflow.instructions.md
│   ├── icl-spec-work.instructions.md
│   ├── parser-development.instructions.md
│   ├── roadmap-tracking.instructions.md
│   ├── rust-development.instructions.md
│   └── testing-determinism.instructions.md
└── skills/                  # AI skill modules (10 skills)
    ├── binding-development/
    ├── brainstorming/
    ├── ci-cd-setup/
    ├── code-review/
    ├── debugging/
    ├── git-workflow/
    ├── phase-execution/
    ├── publishing/
    ├── skill-creator/
    └── using-superpowers/
```

## Instructions

File-pattern-matched coding guidelines that apply automatically:

| File | Applies To | Purpose |
|------|-----------|---------|
| `cli-development` | `**/icl-cli/**`, `**/main.rs` | CLI architecture and UX patterns |
| `documentation` | `**/*.md` | Markdown and documentation standards |
| `git-workflow` | `**` | Commit format, branching, SSH remotes |
| `icl-spec-work` | `**/ICL-Spec/**`, `**/*.icl` | Spec authoring and conformance |
| `parser-development` | `**/parser/**` | Parser architecture and error recovery |
| `roadmap-tracking` | `**/ROADMAP.md` | Milestone tracking and phase workflow |
| `rust-development` | `**/*.rs` | Rust idioms, error handling, testing |
| `testing-determinism` | `**/*test*`, `**/tests/**` | Determinism proofs and test patterns |

## Skills

Specialized AI capabilities triggered by task context:

| Skill | Trigger | Purpose |
|-------|---------|---------|
| `binding-development` | Creating/modifying language bindings | PyO3, wasm-pack, cgo patterns |
| `brainstorming` | Planning features, design decisions | Trade-off evaluation |
| `ci-cd-setup` | GitHub Actions, CI pipelines | Testing, builds, publishing workflows |
| `code-review` | Reviewing code changes | Quality, determinism, spec compliance |
| `debugging` | Investigating bugs or failures | Systematic debugging procedures |
| `git-workflow` | Git operations across repos | Commit format, cross-repo coordination |
| `phase-execution` | Starting/completing ROADMAP phases | Phase workflow enforcement |
| `publishing` | Publishing to registries | crates.io, PyPI, npm releases |
| `skill-creator` | Creating new skills | Skill authoring guide |
| `using-superpowers` | Session startup | Skill discovery and invocation |

## Project Repos

| Repo | Description |
|------|-------------|
| [ICL-Spec](https://github.com/ICL-System/ICL-Spec) | Language specification, grammar, conformance tests |
| [ICL-Runtime](https://github.com/ICL-System/ICL-Runtime) | Rust runtime — parser, normalizer, verifier, executor |
| [ICL-Docs](https://github.com/ICL-System/ICL-Docs) | mdBook documentation site |

## License

MIT
