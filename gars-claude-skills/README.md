# gars-claude-skills

## Goal

This repo open-sources my personal Claude Code skills and the project structure I use to keep multi-session work organized. It exists so others can read, reuse, or adapt the skills directly, and so I have a stable public reference for my own setup. The repo itself is built using the `claude-project-structure` skill it documents, and every doc page in here was written with the help of the skills it describes.

## Continue from here

Status: initial structure is in place - skills copied in, doc pages written, CLAUDE.md rule added. Skills resynced from the global skill set on 2026-08-17: fixed drift in `claude-project-structure` and `code-philosophy`, added `skill-refactoring`. Same day, added a new `agents/` folder holding the 5 global coding agents (code-writer, code-reviewer, code-tester, code-auditor, dependency-installer), each rewritten with a Minimalism-only pass (em-dashes removed, wording otherwise untouched). `doc/example-claude-project/` is intentionally empty.

Outstanding:
1. Populate `doc/example-claude-project/` with a worked example of a project using this structure.
2. Decide whether to add more skills/agents as they're written, or keep this repo as a periodic snapshot (swamp/swamp-getting-started stay excluded either way, see Project structure below).

To resume: pick up item 1, or check `CONTEXT_LOGGING.md` for the latest entry before making changes.

## Table of contents

- [doc/skill-creation.md](doc/skill-creation.md) - the meta-skill used to write every other skill in this repo.
- [doc/other-skills.md](doc/other-skills.md) - one-paragraph summary of every other skill.
- [doc/claude-project-structure.md](doc/claude-project-structure.md) - what the claude-project-structure skill sets up, how this repo uses it, and how to apply it elsewhere.
- [doc/agents.md](doc/agents.md) - the 5 coding agents in `agents/`, what each does, and how the dependency-install pipeline gates between them.
- [CONTEXT_LOGGING.md](CONTEXT_LOGGING.md) - dated history of what changed in this repo and why.
- `doc/example-claude-project/` - not yet populated; will hold a worked example of this project structure in use.

## Project structure

This folder is the internal project doc set; the repo root also has a short public-facing `README.md` and the `LICENSE`.

```
.
├── CLAUDE.md                     rule to keep README/CONTEXT_LOGGING/doc current
├── README.md                     this file
├── CONTEXT_LOGGING.md            dated changelog
├── skills/                       one folder per skill, each a SKILL.md (+ optional references)
│   ├── claude-project-structure/
│   ├── code-philosophy/
│   ├── fact-checking/
│   ├── product-research/
│   ├── professional-communication/
│   ├── skill-creation/
│   ├── skill-refactoring/
│   └── tooling-research/
├── agents/                       one flat file per coding agent, agents/<name>.md
│   ├── code-writer.md
│   ├── code-reviewer.md
│   ├── code-tester.md
│   ├── code-auditor.md
│   └── dependency-installer.md
└── doc/
    ├── skill-creation.md         detail on the meta-skill
    ├── other-skills.md           brief summary of the remaining skills
    ├── claude-project-structure.md  the claude-project-structure pattern, and how to use it
    ├── agents.md                 the 5 coding agents and how they gate on each other
    └── example-claude-project/   empty, reserved for a future worked example
```

`swamp` and `swamp-getting-started` are excluded from `skills/` on purpose: they reference an internal/company CLI and are not relevant to this project. Do not add them when syncing from the global skill set. `agents/` follows the same rule: only agents relevant to this project's own use are ported in, and the global `~/.claude/agents/` source is never written to from here.
