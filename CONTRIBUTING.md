# Contributing to Session Continuity Skill

Thanks for wanting to improve this. Here's how.

---

## Ways to Contribute

- **Bug reports** — Open an issue describing what happened vs. what you expected
- **Trigger improvements** — Found a phrase that should activate the skill but doesn't? Open an issue or PR
- **Edge case handling** — New situations the skill doesn't handle gracefully
- **Translations** — Adapting trigger phrases for non-English users
- **New variants** — E.g. a "weekly digest" mode or "project-focused" mode

---

## How to Submit a PR

1. Fork the repo
2. Edit `skill/SKILL.md` with your improvement
3. Repackage the `.skill` file (it's a zip — rename `.skill` to `.zip` to inspect)
4. Submit a PR with:
   - What you changed and why
   - Example before/after of the skill's output (if relevant)

---

## Skill File Format

The `.skill` file is a ZIP archive containing the `session-continuity/SKILL.md`.
You can open it with any zip tool to inspect or modify.

To repackage after editing:
```bash
cd skill/
zip -r session-continuity.skill session-continuity/
```

---

## Code of Conduct

Be constructive. Be specific. Be kind.
