# Context logging

Append-only. Newest entry at the bottom. Never edit old entries; append corrections instead.

---

## 2026-08-06

Repo initialized as a Claude project per the `claude-project-structure` skill.

- Copied 7 skills into `skills/`: claude-project-structure, code-philosophy, fact-checking, product-research, professional-communication, skill-creation, tooling-research. Excluded `swamp` and `swamp-getting-started` since they reference an internal/company CLI tool and aren't fit for open-sourcing.
- Created `doc/skill-creation.md` (detail on the meta-skill) and `doc/other-skills.md` (brief summary of the rest).
- Created empty `doc/example-claude-project/`, reserved for a future worked example, at the user's request.
- Wrote README.md following the four-section structure (Goal, Continue from here, TOC, Project structure) from `claude-project-structure`, in the voice specified by `professional-communication`.
- Updated the global `claude-project-structure` skill (`~/.claude/skills/claude-project-structure/SKILL.md`) per `skill-creation` principles: added a Principals section, split the changelog out of README.md into a separate CONTEXT_LOGGING.md, and added a worked example. Synced the updated SKILL.md into this repo's `skills/claude-project-structure/`.
- Added a Documentation section to this repo's CLAUDE.md pointing at README.md, CONTEXT_LOGGING.md, and doc/.
