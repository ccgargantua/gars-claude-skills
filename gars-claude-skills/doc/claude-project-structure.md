# Project structure and the claude-project-structure skill

Location of the skill: `skills/claude-project-structure/SKILL.md`

## What it sets up

For any multi-session project, the skill creates:

```
.
├── CLAUDE.md          (or AGENTS.md)
├── README.md
├── CONTEXT_LOGGING.md
└── doc/
    └── <topic>.md
```

- **README.md** - four sections only: Goal, Continue from here, Table of contents, Project structure. No changelog. If it stops changing between sessions, the pattern has failed.
- **CONTEXT_LOGGING.md** - the changelog. Append-only, dated, newest entry at the bottom. Never edit old entries; append corrections.
- **doc/** - one file per topic, each linked from the README TOC. A doc page not linked from the TOC is effectively invisible to whoever reads the project next.
- **CLAUDE.md / AGENTS.md** - carries a short rule telling the agent to read README at the start of every session, and to update README, CONTEXT_LOGGING.md, and the relevant doc page on any change to project state.

## How this repo uses it

`gars-claude-skills/` is that structure, applied to itself:

- [`README.md`](../README.md) - goal, current status, TOC, structure.
- [`CONTEXT_LOGGING.md`](../CONTEXT_LOGGING.md) - dated history of every change made to this repo.
- `doc/` - this file, plus `skill-creation.md`, `other-skills.md`, and the empty `example-claude-project/` placeholder.
- [`CLAUDE.md`](../CLAUDE.md) - the documentation rule, unchanged from the skill's template.

## How to apply it to a new project

1. Invoke the `claude-project-structure` skill.
2. Answer honestly when it asks what the project's unit of change is (files, services, pipeline stages, whatever actually changes session to session). The skill will not guess this for you.
3. Let it create the four files/folders above.
4. From then on, treat README.md's "Continue from here" section as the source of truth for where things stand, and CONTEXT_LOGGING.md as the record of how it got there.

## Common mistakes the skill guards against

- Putting the changelog in README.md, which buries current status under history.
- Letting "Continue from here" go stale past the point where it's still useful to resume from.
- Creating empty topic files in `doc/` before there's anything to say.
- Leaving a doc page unlinked from the TOC.
