---
name: skill-refactoring
description: "Audit one or more existing Claude Code skills (SKILL.md files) against the skill-creation skill's standard, and bring non-compliant ones into compliance without losing any rule, example, table row, code block, or trigger phrase from the original. Use when asked to audit, clean up, refactor, standardize, or review skills, check them for em-dashes/emojis/filler/missing Principals or Example sections, or bring skills up to spec with skill-creation."
---

# Principals

1. Principals - This list governs how the skill is followed. Re-check every step below against these principals before editing any skill file.
2. Minimalism - No em-dashes, no emojis, no filler. Fixes should be the smallest edit that achieves compliance, not a rewrite for its own sake.
3. Strict Validation - Never assume a skill is compliant or non-compliant from memory of a past audit. Re-read skill-creation's current text and the full target file fresh every time; both may have changed since the last pass.
4. Degrees of Freedom - Never overwrite a skill file without the original content already captured in context to diff against afterward. Never add anything to a shared file outside the skill being audited (a global CLAUDE.md, another skill) without confirming with the user first.

# Process

1. Load the skill-creation skill fresh. Treat its current text as the standard, not a remembered summary of it.
2. Confirm scope with the user if ambiguous: global skills (`~/.claude/skills`), project skills (`.claude/skills`), or a named subset. Don't assume "all of them" without checking when the request is vague.
3. For each skill, read the full SKILL.md (and any file it directly reads as its own content, not deep reference trees) before making any edit. This read is the audit baseline, keep it in context for the whole pass.
4. Score the file against skill-creation's requirements:
   - A Principals section is present, containing at minimum Principals, Minimalism, and Strict Validation, worded to fit the skill's own domain rather than pasted boilerplate.
   - Degrees of Freedom is included when the skill can trigger consequential, destructive, or irreversible actions.
   - An Example section exists: a fake user question, answered by actually applying the skill's own rules.
   - Zero em-dashes, zero emojis, no padding or restated filler.
   - The frontmatter description still contains the trigger words/phrases it had before, so routing behavior doesn't silently change.
5. Classify each file: compliant (no change needed, or a one-line formatting fix) or non-compliant (missing Principals and/or Example, or contains forbidden characters).
6. For a one or two line fix (a stray em-dash, an emoji), make a targeted edit, not a full rewrite. Cheaper in tokens and lower risk of an accidental change elsewhere in the file.
7. For a structural fix, write a full replacement, but build it from the original: keep every existing sentence, rule, table row, code block, and link verbatim, and only (a) insert the Principals block, (b) append an Example if missing, (c) swap forbidden characters for equivalent plain punctuation (em-dash to comma, colon, or plain hyphen, matching local grammar). Never shorten, merge, or cut an existing rule or example to save space, brevity is not the goal here, correctness against the baseline is.
8. After every edit, audit the result against the captured baseline, section by section. Every baseline sentence, rule, and example must still be present, with punctuation as the only allowed difference. If something genuinely cannot be preserved verbatim, say so explicitly and why, rather than dropping it silently.
9. If something in skill-creation looks like a general behavior rule rather than a skill-writing rule (for example, a rule that would make sense as a standing instruction outside of any one skill), flag it as a candidate for the relevant CLAUDE.md and ask the user before adding it. Don't edit CLAUDE.md unprompted.
10. Close with a summary: which files were already compliant, which were changed and how (formatting fix vs structural rewrite), a line-count delta per changed file, and explicit confirmation that the post-edit audit found no lost content.

# Example

User: "Run a refactor pass on my global skills against skill-creation."

Skill applied: load skill-creation fresh, confirm scope is `~/.claude/skills`, read every SKILL.md in full before touching any of them, then classify.

"Loaded skill-creation. Scope: all 9 skills under `~/.claude/skills`. Reading each in full first.

Results:
- `fact-checking`, `product-research`: already compliant, no changes.
- `claude-project-structure`: one stray em-dash in a heading, fixing with a targeted edit.
- `code-philosophy`: missing the Principals section entirely (has a "Priorities" list instead, which is domain content, not the required meta-block). Inserting a 3-line Principals block above it, everything else stays byte-identical.

Proceeding with the two fixes above. After each, I'll diff the result against what I read just now and confirm nothing besides the intended change moved."

[Applies the fixes, then reports:]

"Done. `claude-project-structure`: 1 line changed, 0 lines added or removed, only the em-dash. `code-philosophy`: +5 lines (the new Principals block), every other line matches the baseline read verbatim, confirmed by re-reading the file after the edit. No content lost."
