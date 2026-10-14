---
worth: yes
added: 2026-10-13
---
# plugin SKILL.md files never mention --word-diff

`CLAUDE.md` requires every new CLI flag to get a `SKILL.md` entry, so agents launching revdiff know when
to pass it. None of the three copies mentions `--word-diff`: `.claude-plugin/skills/revdiff/SKILL.md`,
`plugins/codex/skills/revdiff/SKILL.md`, `plugins/pi/skills/revdiff/SKILL.md`. The flag is documented
only in `references/config.md` and `references/usage.md`, so an agent never passes it, even for a review
of a refactor where intra-line highlighting is what makes the change readable.

Found while investigating #389 (moved-line highlighting).
