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

Added a new `agents/` folder, ported from the 5 global coding agents (`~/.claude/agents/`), global files untouched.

- Agents are a different artifact from skills: flat `agents/<name>.md` files with `name`/`description`/`tools`/`model` frontmatter, not a `SKILL.md`-per-folder structure. Confirmed placement (top-level `agents/`, flat) with the user before creating it.
- Ported code-writer, code-reviewer, code-tester, code-auditor, dependency-installer. These form a pipeline: writer/reviewer/tester never install dependencies, they report the need; code-auditor renders an APPROVE/REFUSE verdict; dependency-installer only acts once it has an exact package spec, an auditor verdict, and explicit user sign-off, treated as independent gates.
- Applied a Minimalism-only rewrite (per the user's global no-em-dash rule, using skill-refactoring's approach): swapped every em-dash for a comma or colon matching local grammar, left wording otherwise untouched. Did not add a Principals or Example section since those are skill-creation conventions for SKILL.md, not part of Claude Code's agent frontmatter format, at the user's direction.
- Verified via word-level diff against each global source that no content changed beyond the em-dash swap. Also caught and fixed an over-eager first pass that had accidentally flattened three `→` arrows in dependency-installer.md into colons; restored them since arrows aren't covered by the no-em-dash rule.
- Added `doc/agents.md`, linked from the README TOC. Updated README's project structure tree and CLAUDE.md's documentation rule to cover agents alongside skills.

## 2026-08-19

Corrected a long-standing misspelling: "Principals" was used throughout the skill set where "Principles" was meant. Fixed globally first, then synced into this repo.

- Global (`~/.claude/skills/`), all 11 SKILL.md files: `Principals` -> `Principles`, `principals` -> `principles`, `principal` -> `principle`. Covers american-sound, claude-project-structure, code-philosophy, fact-checking, product-research, professional-communication, skill-creation, skill-refactoring, swamp, swamp-getting-started, tooling-research. Backed the global tree up to the session scratchpad first, since `~/.claude/` is not version controlled, then verified the diff was spelling-only before moving on.
- Deliberately left `swamp/references/serve/guide.md` alone: its four "principal(s)" hits are the security/identity sense (`--principal user:<id>`, access-group members), which is correct usage, not a misspelling.
- Two other real typos found by an aspell sweep and fixed globally: `!00% confidence` -> `100% confidence` in skill-creation, and `excess verbage` -> `excess verbiage` in tooling-research.
- Left alone as user voice or verbatim example text rather than errors: "overfluffing" and "shitscape" in professional-communication's sample article, and the citation artifacts in tooling-research's worked example ("DocsMicrosoft Learn", "LearnGitHub", "StreamlitPeerDH"). These are pasted example output; changing them would rewrite the example rather than correct a spelling.
- Project skills: all 8 resynced from global and now match byte for byte. Seven of them (claude-project-structure, fact-checking, product-research, professional-communication, skill-creation, skill-refactoring, tooling-research) differed only by the spelling fixes.
- `code-philosophy` had drifted further: global had been condensed by roughly 30% in a Minimalism pass that reworded nearly every bullet without adding or removing a rule. Confirmed with the user before pulling it in, and did a full resync rather than a spelling-only patch, so the repo keeps mirroring global exactly.
- Doc pages updated for the rename: `doc/skill-creation.md` (section heading and all four principle descriptions), `doc/other-skills.md` (skill-refactoring entry), `doc/agents.md` (Origin note). Older entries in this log keep the original "Principals" spelling since this file is append-only.
- `agents/` needed no change. Verified the 5 project agents still diverge from `~/.claude/agents/` by em-dashes only, as recorded on 2026-08-17, and an aspell sweep found no misspellings in them.
- Noted that `american-sound` is a new global skill since the last sync. It stays excluded from this repo for the same reason as swamp: it documents an employer's internal systems. Recorded that in README's exclusion rule alongside the swamp skills so future syncs don't re-raise it.
