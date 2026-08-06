# gars-claude-skills

These are my Claude Code skills, the actual ones I use day to day, opened up for anyone who wants to read them, steal from them, or tell me I've got something wrong.

I built these because I got tired of AI output that sounds confident and turns out to be padded, vague, or just made up. Every skill here exists to fight one of those failure modes directly, not to look impressive in a demo.

Everything lives in [`gars-claude-skills/`](gars-claude-skills/): the skills themselves in `skills/`, the documentation in `doc/`. Start with [`gars-claude-skills/README.md`](gars-claude-skills/README.md) for where things actually stand, and [`doc/claude-project-structure.md`](gars-claude-skills/doc/claude-project-structure.md) if you want to understand how the whole thing is organized and why.

## Philosophy

None of these skills were written in isolation - they all answer to the same four rules, spelled out in full in [`skill-creation`](gars-claude-skills/skills/skill-creation/SKILL.md):

1. **Principals govern the skill** - every skill states, up front, how it wants to be followed. No guessing at intent.
2. **Minimalism** - cut the filler. No em-dashes, no emojis, no padding a short answer to look thorough. Tokens cost time, mine and the model's.
3. **Strict validation** - if it can't be verified, it doesn't get stated as fact. Ask instead of guessing, every time.
4. **Degrees of freedom** - decide up front what the model is allowed to do unsupervised, and confirm it with me. Don't leave that implicit and hope it works out.

None of this is decoration. It's the difference between output I can trust without babysitting it and output I have to double-check anyway.

Licensed under [MIT](LICENSE).
