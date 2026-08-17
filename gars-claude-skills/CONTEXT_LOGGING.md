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

Moved everything internal (CLAUDE.md, CONTEXT_LOGGING.md, README.md, doc/, skills/) into a `gars-claude-skills/` subfolder, and added a short public-facing README.md at repo root pointing into it, since the root README should read like an actual repo README rather than the internal project doc.

- Added doc/claude-project-structure.md (initially named project-structure.md, renamed at the user's request), explaining the claude-project-structure pattern, how this repo applies it, and how to reuse it elsewhere. Linked from the README TOC.

## 2026-08-17

Resynced project skills against the current global skill set (`~/.claude/skills/`), global files untouched.

- `skills/claude-project-structure/SKILL.md`: fixed a stray em-dash that had crept into a heading.
- `skills/code-philosophy/SKILL.md`: restored a missing "Principals" section (Principals/Minimalism/Strict Validation) that global had gained since the last sync.
- `fact-checking`, `product-research`, `professional-communication`, `skill-creation`, `tooling-research`: already matched global, no changes.
- Added `skills/skill-refactoring/` (new in global since the last sync), at the user's request. Documented in README's project structure tree and doc/other-skills.md.
- Confirmed with the user that `swamp` and `swamp-getting-started` stay permanently excluded (internal/company tool, not relevant here) and recorded that as a standing rule in README rather than only in this log, so future syncs don't re-raise the question.
