# Gargantua's Claude Skills

These are my Claude Code skills. I built these because I got tired of AI output that sounds confident and turns out to be padded, vague, or just made up. Every skill here exists to fight one of those failure modes directly.

This project was made using the [project creation skill](./gars-claude-skills/doc/claude-project-structure.md). For that reason, the [README.md](./gars-claude-skills/README.md) might not make sense at first.

## Philosophy

As remarkable a technology AI is, it is quite good at slowing itself down and, even worse, slowing you down. Claude tends to pollute its own context with repetitive mistakes, filler text, and reading content that it has already processed in the past. These skills are written with the attention of addressing those pain points directly.

Three principles govern everything the skills produce:

1. **Never assume anything.** Turn every assumption into a question.
2. **Know when a task is complete.** Don't extend scrutiny to infinity in a perfectionist loop.
3. **Density.** LLM tokens and the user's time are precious resources. Responses should be as dense as possible without sacrificing any technical detail. Every extra character, word, and sentence is a waste.

The skills deliberately do not restate those principles. They live in [gars-claude-skills/CLAUDE.md](gars-claude-skills/CLAUDE.md), which Claude loads on every turn, so repeating them inside a skill would cost tokens per turn and change nothing. If you adopt these skills elsewhere, copy that principles block into the CLAUDE.md of wherever they land, or they inherit nothing.

All of these skills were made using the [skill-creation skill](gars-claude-skills/doc/skill-creation.md), which also fixes the parts of a skill no principle can enforce: consequential actions gated in frontmatter rather than prose, degrees of freedom confirmed with me before drafting, and a worked example in every skill.

Licensed under [MIT](LICENSE).
