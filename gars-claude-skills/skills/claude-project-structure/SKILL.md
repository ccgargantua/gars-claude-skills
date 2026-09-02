---
name: claude-project-structure
description: "Use when starting a new multi-session project, or when asked to set up persistent project documentation, a knowledgebase, or context continuity for Claude/agents. Sets up README.md (goal, resume section, TOC, structure) + CONTEXT_LOGGING.md (dated changelog) + doc/ (one file per topic), plus CLAUDE.md/AGENTS.md rules to read/update them each session. Triggers on 'set up project docs', 'persistent context', 'project knowledgebase', 'contextual persistence', 'continue from here', 'session continuity', 'resumable project'."
---

# Rules

1. README and doc pages exist to be read under time pressure; every extra word costs the next reader time.
2. Never assume the project's unit of change, its topic breakdown, or what counts as "done." Ask the user when unclear instead of guessing.
3. Before creating files or folders beyond the fixed skeleton below, confirm naming and scope with the user rather than inventing structure unprompted.

# Structure

```
.
├── CLAUDE.md          (or AGENTS.md)
├── README.md
├── CONTEXT_LOGGING.md
└── doc/
    └── <topic>.md
```

# README.md - four sections, in order

1. **Goal** - one paragraph, what the project is for.
2. **Continue from here** - rewritten (not appended) whenever stale:
   - current status, 1-2 sentences
   - numbered outstanding items, actionable without other context
   - the concrete command/step to resume
3. **Table of contents** - one line per `doc/` page, what it covers.
4. **Project structure** - annotated file tree, one-line purpose per dir.

README never holds the changelog. That belongs in CONTEXT_LOGGING.md, kept separate so status-at-a-glance isn't buried under history.

# CONTEXT_LOGGING.md

- Append-only, dated, newest entry at the bottom.
- Each entry: what was tried, found, decided, why, outcome.
- Never edit old entries; append corrections instead.
- Linked from the README TOC like any other doc page.

# doc/

- One file per topic, linked from the README TOC. Unlinked = invisible.
- Depth that would bloat README: architecture, gotcha references, external-system conventions.
- Don't pre-create empty topic files.

# CLAUDE.md / AGENTS.md rule to add

```markdown
## Documentation

`README.md` is the canonical, always-current record of project state.
`CONTEXT_LOGGING.md` holds the dated history of what was done and why.
Deeper topics live under `doc/`, one file per section, linked from the
README TOC.

- Read `README.md` at the start of every session.
- On any change to project state, update README's "Project structure",
  append an entry to `CONTEXT_LOGGING.md`, and add/update the relevant
  `doc/*.md` page.
- Rewrite "Continue from here" whenever it goes stale, not just at session end.
- Periodically check README for staleness even with no specific change.
```

Adapt "project state" to the project's real unit of change (files, infra, services, modules). Confirm this unit with the user rather than assuming it.

# Setup steps

1. Ask the user to identify the project's unit of change if it isn't obvious.
2. Create README.md with the four sections.
3. Create CONTEXT_LOGGING.md with one entry: "repo initialized".
4. Create `doc/` (empty).
5. Add the Documentation section to CLAUDE.md/AGENTS.md, adapted.

# Don'ts

- Don't let README go static - if it stops changing, the pattern failed.
- Don't put changelog entries in README - they belong in CONTEXT_LOGGING.md.
- Don't bury "Continue from here" inside history.
- Don't leave doc/ pages unlinked from the TOC.
- Don't rewrite old CONTEXT_LOGGING.md entries - append corrections.

# Example

User: "Set up persistent project docs for this repo, it's a Python ETL pipeline I'll be working on across sessions."

Skill applied: unit of change is unclear from that alone (files? pipeline stages? data sources?), so ask before assuming:

"Before I set this up: what's the right unit of change to track here - individual pipeline stages, data source integrations, or something else? And is this a fresh repo or does it already have a README I'd be replacing?"

Once answered, create README.md (Goal / Continue from here / TOC / Project structure), CONTEXT_LOGGING.md with a single "repo initialized" entry, an empty `doc/`, and the Documentation section in CLAUDE.md - with "project state" phrased in terms of the confirmed unit of change.
