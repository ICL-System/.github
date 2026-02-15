---
applyTo: "**/*.md"
---

# Documentation — ICL

## Honesty Policy

- Never document features that don't exist as if they work
- Status badges must reflect actual current phase
- Code examples must actually work (or be clearly marked "planned")
- Install commands only shown when packages actually exist

## Cross-Repo Links

Always use absolute GitHub URLs for cross-repo references:
- `https://github.com/ICL-System/ICL-Spec`
- `https://github.com/ICL-System/ICL-Runtime`
- `https://github.com/ICL-System/ICL-Docs`

## After Any Change

- Verify `README.md` in all 3 repos stays in sync
- Run `mdbook build` if editing ICL-Docs
- Update ICL-Runtime/ROADMAP.md status if completing a phase item
