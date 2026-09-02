---
name: skill-refactoring
description: "Audit one or more existing Claude Code skills (SKILL.md files) against the skill-creation skill's standard, and bring non-compliant ones into compliance without losing any rule, example, table row, code block, or trigger phrase from the original. Use when asked to audit, clean up, refactor, standardize, or review skills, check them for em-dashes/emojis/filler/restated principle boilerplate/ungated side effects/missing Rules or Example sections, or bring skills up to spec with skill-creation."
---

# Rules

1. Re-read skill-creation's current text and the full target file fresh every audit. Never assume a skill is compliant or non-compliant from memory of a past pass; both files may have changed.
2. Never overwrite a skill file without the original content already captured in context to diff against afterward.
3. Never add anything to a shared file outside the skill being audited (a global CLAUDE.md, another skill) without confirming with the user first.
4. Fixes are the smallest edit that achieves compliance, not a rewrite for its own sake.

# Process

1. Load the skill-creation skill fresh. Treat its current text as the standard, not a remembered summary of it.
2. Confirm scope with the user if ambiguous: global skills (`~/.claude/skills`), project skills (`.claude/skills`), or a named subset. Don't assume "all of them" without checking when the request is vague.
3. For each skill, read the full SKILL.md (and any file it directly reads as its own content, not deep reference trees) before making any edit. This read is the audit baseline, keep it in context for the whole pass.
4. Score the file against skill-creation's requirements:
   - Description: key use case first, contains the trigger phrases users type, combined limit 1,536 characters, and it still contains every trigger word or phrase it had before the edit, so routing behavior doesn't silently change.
   - No restated shared principles (Minimalism or Strict Validation boilerplate). Domain-specific constraints live under a "# Rules" heading; generic principle text is removed, never the domain content.
   - Side effects gated in frontmatter where a field can enforce them (disable-model-invocation, allowed-tools, user-invocable, paths, context). Constraints no field can express stay as body Rules.
   - A "# Example" section exists: a fake user question, answered by actually applying the skill's own rules.
   - Zero em-dashes, zero emojis, no padding or restated filler.
   - Under 500 lines. Moving bulk to references/ changes file layout, so propose it to the user rather than doing it silently.
5. Classify each file: compliant (no change needed, or a one-line formatting fix) or non-compliant.
6. For a one or two line fix (a stray em-dash, an emoji, a leftover boilerplate line), make a targeted edit, not a full rewrite. Cheaper in tokens and lower risk of an accidental change elsewhere in the file.
7. For a structural fix, write a full replacement, but build it from the original: keep every existing sentence, rule, table row, code block, and link verbatim. The only allowed removals are generic principle boilerplate and forbidden characters (em-dash to comma, colon, or plain hyphen, matching local grammar). Never shorten, merge, or cut a domain rule or example to save space; correctness against the baseline is the goal, not brevity.
8. After every edit, audit the result against the captured baseline, section by section. Every baseline domain sentence, rule, and example must still be present, with boilerplate removal and punctuation as the only allowed differences. If something genuinely cannot be preserved verbatim, say so explicitly and why, rather than dropping it silently.
9. If something in a skill looks like a general behavior rule rather than domain content (a rule that would make sense as a standing instruction outside any one skill), flag it as a candidate for the relevant CLAUDE.md and ask the user before adding it. Don't edit CLAUDE.md unprompted.
10. Close with a summary: which files were already compliant, which were changed and how (formatting fix vs structural rewrite), a line-count delta per changed file, and explicit confirmation that the post-edit audit found no lost domain content.

# Example

User: "Run a refactor pass on my global skills against skill-creation."

Skill applied: load skill-creation fresh, confirm scope is `~/.claude/skills`, read every SKILL.md in full first, then classify.

"Loaded skill-creation. Scope: all 10 skills under `~/.claude/skills`. Reading each in full first.

Results:
- `tooling-research`, `product-research`: already compliant, no changes.
- `claude-project-structure`: one stray em-dash in a heading, fixing with a targeted edit.
- `fact-checking`: still restates Minimalism boilerplate above its domain rules. Removing those two lines; the domain rules stay byte-identical.
- `deploy-helper`: can trigger a production deploy but has no disable-model-invocation. Proposing that gate; confirm before I add it since it changes when the skill fires.

Proceeding with the first three fixes. After each, I'll diff the result against the baseline read and confirm nothing besides the intended change moved."

[Applies the fixes, then reports:]

"Done. `claude-project-structure`: 1 line changed, only the em-dash. `fact-checking`: -2 lines, both generic boilerplate, every domain rule verbatim against the baseline, confirmed by re-reading after the edit. `deploy-helper` gate awaiting your confirmation. No domain content lost."
