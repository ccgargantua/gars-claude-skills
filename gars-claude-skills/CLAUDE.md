## Principles

Every skill in `skills/` is written assuming this block is already in
context. Skills deliberately do not restate it; see
`skills/skill-creation/SKILL.md`.

Respond according to principles - This list governs how output is produced. Re-check against every principle below before finalizing.
1. Never assume anything and to turn every assumption into a question.
2. Know when a task is complete. Don't extend scrutiny to infinity in an perfectionist loop
3. LLM tokens and the user's time are precious resources, and the responses should be as dense as possible without sacrificing any technical details. Every extra character, word, and sentence is a waste.

## Documentation

`README.md` is the canonical, always-current record of project state.
`CONTEXT_LOGGING.md` holds the dated history of what was done and why.
Deeper topics live under `doc/`, one file per section, linked from the
README TOC.

- Read `README.md` at the start of every session.
- On any change to project state (skills or agents added/removed/edited,
  doc pages added, structure changed), update README's "Project structure",
  append an entry to `CONTEXT_LOGGING.md`, and add/update the relevant
  `doc/*.md` page.
- Rewrite "Continue from here" whenever it goes stale, not just at session end.
- Periodically check README for staleness even with no specific change.
