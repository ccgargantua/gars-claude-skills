---
name: skill-creation
description: "Create a Claude Code skill (SKILL.md) to the current house standard. Use when asked to create a skill, write a skill, generate a SKILL.md, make a slash command, or turn repeated instructions or a CLAUDE.md section into a skill."
argument-hint: "[what the skill should do]"
---

This file is the standard for every skill in this collection. skill-refactoring audits against it.

# Shared principles (canonical wording)

Every skill inherits these at runtime from the org instructions and the global CLAUDE.md, which are always in context. Do NOT restate them inside a new skill; restating is a per-turn token cost with no behavior change.

1. Minimalism - No em-dashes, no emojis, no filler. A loaded skill body is a recurring cost every turn. Keep a line only if removing it would cause mistakes.
2. Strict Validation - Never state a technical fact (command, flag, API behavior, price, spec) that has not been verified in this conversation. Unknowns become questions to the user, never gap-fills from training data.

# Structure

```
skill-name/
  SKILL.md        (under 150 lines preferred, 500 hard max)
  references/     (optional detail, loaded on demand)
  scripts/        (optional, executed via ${CLAUDE_SKILL_DIR}, never loaded as context)
```

# Frontmatter

The description is the only text Claude sees when deciding whether to load the skill. Key use case first, then "Use when" with the literal phrases users type. Combined description plus when_to_use limit: 1,536 characters. If a sibling skill overlaps, state what this one is NOT for.

Gate side effects with frontmatter, not prose, wherever a field can enforce the constraint:

| Skill can trigger                        | Set                            |
|------------------------------------------|--------------------------------|
| deploy, send, commit, delete, publish    | disable-model-invocation: true |
| work needing only specific tools         | allowed-tools                  |
| background knowledge, never a command    | user-invocable: false          |
| work relevant only to certain files      | paths (globs)                  |
| long noisy work that pollutes context    | context: fork                  |

List every consequential action the new skill can take, propose gates, and confirm with the user which freedoms the skill keeps. A constraint no field can express (e.g. "confirm the target before a destructive subcommand") stays in the body as a domain rule.

# Body rules

1. Write for the smallest model that will run it (Haiku): short imperative lines, one rule per line, most binding rules first. No meta-instructions like "re-check everything above"; small models spend those tokens without gaining accuracy.
2. State what to do, not how or why. If a reason is essential, one clause: "Use X because Y."
3. Include only non-obvious content: domain gotchas, house conventions, exact commands. Never restate default Claude behavior or the shared principles.
4. Domain-specific constraints go under a "# Rules" heading near the top, worded for the domain.
5. Add a "# Gotchas" section when the domain has known traps. It is the highest-value content in a skill.
6. Move bulky material (API tables, long references) to references/ and link it from the body.
7. End with a "# Example" section: one realistic user message and the response produced by applying the skill. Concrete examples are what small models imitate best.

# Authoring process

1. Ask the user, in one round: purpose, the phrases they will use to trigger it, every consequential action it may take, and any facts you cannot verify yourself.
2. Draft frontmatter and body per the rules above.
3. Verify every command, flag, and fact stated in the skill in this conversation, or leave it as an explicit question to the user.
4. Final check: description contains the user's trigger phrases; no em-dashes or emojis; within line budget; side effects gated in frontmatter; Rules and Example sections present.
5. Show the draft to the user for review, then have them test in a fresh session: run 2 or 3 target prompts with the skill and without it, and compare. Authoring-session context masks gaps a clean session exposes.

# Example

User: "Make me a skill for writing SQL migrations at work."

Skill applied, one round of questions before drafting:

"Three things before I write it:
1. Trigger phrases: do you say 'migration', 'schema change', or something else? The description needs your exact words.
2. Consequential actions: should the skill ever run migrations, or only write the files? If it can run them, I'll set disable-model-invocation: true so it only fires when you invoke it.
3. Facts I can't verify: your migration file naming convention and which database engine."

After answers: draft the skill with a Rules section for the naming convention, a Gotchas section for engine-specific traps the user confirms, an Example, and the agreed frontmatter gates. Then: "Test it in a fresh session with 2 or 3 real migration requests, with and without the skill, and tell me where it misfires."
