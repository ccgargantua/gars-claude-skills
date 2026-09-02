---
name: writing-documentation
description: "Write technical documentation that is comprehensively dense, atomic, and navigable. Use whenever asked to write, edit, review, or restructure documentation of any kind: wikis, READMEs, process docs, concept explanations, or internal standards."
---

Principles:
1. Principles - Principles are a vital part of every skill. They describe to the agent how the skill should be followed. Every skill should include a list of principles, similar to this one.
2. Minimalism - No extra wording, no em-dashes, no emojis. Tokens are a precious resource and should be respected as such. LLMs do not care about fancy formatting, it is not necessary and is wasteful. For documentation specifically: Employ a refined practice of comprehensive density. Verbosity is important only in conveying technical details, but excessive prose is counter productive and wasteful. LLM tokens and the reader's time are both precious resources that must be conserved. No filler, no emojis, no excessive punctuation or prose. Be direct and spare no technical details.
3. Strict Validation - If documentation states technical information or objective claims, these MUST BE VALIDATED! Assumptions SHOULD NOT EVER BE MADE! Do not fill in gaps or blanks; ask the user to fill in those blanks for you. Do not rely on training data for information you cannot confirm with 100% confidence. Never invent commands, paths, versions, URLs, or behavior.

Rules for writing documentation:

1. Reader assumptions. Write as if the reader has never read this documentation before, including for the process, tool, or standard at hand, and as if the reader is in an urgent rush to find what they need. Every word costs reading time; every missing technical detail causes confusion, which costs more time. Balance the reader's time against the reader's understanding.

2. Atomic knowledge. Break information down to its smallest isolated components. A document explaining an atomic concept must itself be small. Example: a Git commit can be explained without branches, merging, or stashing; do not drag those in.

3. Accessibility and structure. Choose structure by granularity:
   - One atomic concept that stands alone: its own page.
   - Several small, connected atomic concepts: separate sections on one page so the reader can bounce between them.
   Documentation ends up as a bookmark someone references repeatedly. Make it digestible and easy to navigate.

4. Processes and complex concepts. When a topic cannot be feasibly reduced to isolated atomic documents (a process like Trunk-Based Development, a concept like branching), build a hierarchy of encapsulation: either one document with multiple sections or a top-level page linking to subpages. Navigation is key. Include a table of contents or subpage links, especially for frequently referenced documents.

5. Repetition and linking. Referencing other documented concepts is natural. A repeated statement must never exceed one sentence. Anything longer becomes a link to the other document or section. If the repetition does not save the reader more time than clicking a link, do not repeat it.

6. Validation before writing. Before documenting any technical detail not already established in context (tool behavior, commands, configuration, terminology, ownership), ask the user or verify against an authoritative source. List open questions to the user rather than guessing.

Example:

User: "Write internal documentation explaining what a Git commit is."

Response following this skill:

# Commits

A commit is a snapshot of your repository at a point in time. Each commit records:
- The full state of all tracked files
- The author, timestamp, and a message describing the change
- A unique SHA-1 hash identifying it
- A pointer to its parent commit(s)

Create a commit:

```
git add <files>
git commit -m "message"
```

Commits are local until pushed. See [Branches] and [Merging] for how commits relate across lines of development.

(Note the example does not explain branches or merging; those are separate atomic concepts, referenced by link only.)
