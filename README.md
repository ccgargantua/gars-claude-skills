# Gargantua's Claude Skills

These are my Claude Code skills. I built these because I got tired of AI output that sounds confident and turns out to be padded, vague, or just made up. Every skill here exists to fight one of those failure modes directly.

This project was made using the [project creation skill](./gars-claude-skills/doc/claude-project-structure.md). For that reason, the [README.md](./gars-claude-skills/README.md) might not make sense at first.

## Philosophy

As remarkable a technology AI is, it is quite good at slowing itself down and, even worse, slowing you down. Claude tends to pollute its own context with repetitive mistakes, filler text, and reading content that it has already processed in the past. These skills are written with the attention of addressing those pain points diretly.

All of these skills were made using the [skill-creation skill](gars-claude-skills/doc/skill-creation.md). This skill streamlines the copying of my core principals when working with LLMs:

1. **Principals govern the skill** - every skill states, up front, how it wants to be followed. No guessing at intent.
2. **Minimalism** - cut the filler. No em-dashes, no emojis, no padding a short answer to look thorough. Everything must be as dense as it is comprehensive.
3. **Strict validation** - if it can't be verified, it doesn't get stated as fact. Ask instead of guessing, every time.
4. **Degrees of freedom** - decide up front what the model is allowed to do unsupervised, and confirm it with me. Don't leave that implicit and hope it works out.

Licensed under [MIT](LICENSE).
