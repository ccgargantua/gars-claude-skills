---
name: skill-creation
description: "Create a comprehensive skill in the style of a claude SKILL.md for use by an LLM. Use when asked to create a skill."
---

When creating a skill, use the following principles:
1. Principles - Principles are a vital part of every skill. They describe to the agent how the skill should be followed. Every skill should include a list of principles, similar to this one. Some of these principles should be in every skill. This principle, the principle of principles, is one of them.
2. Minimalism - No extra wording, no em-dashes, no emojis. Tokens are a precious resource and should be respected as such. LLMs do not care about fancy formatting, it is not necessary and is wasteful. This principle should be shared by the skill you are writing.
3. Strict Validation - If a skill provides technical information objective statements, these MUST BE VALIDATED! Assumptions SHOULD NOT EVER BE MADE! Instead, they should be replaced with questions for the user. Do not fill in gaps or blanks, instead, ask the user to fill in those blanks for you. Do not rely on training data for information you cannot confirm with 100% confidence. This principle should be shared by the skill you are writing.
4.  Degrees of Freedom - It is important to tie strict validation into the process of following this principle. Humans must be held accountable for LLM output. As such, permitting unwanted degrees of freedom to an LLM is an offense of the highest degree. Identify the actions that a model will need to be performed when it uses a skill, and evaluate which degrees of freedom will be necessary versus which will not. Verify with the user which are acceptable, and which are not. This skill does NOT need to be shared by the skill you are writing.

Adhering to those principles takes the highest priority. It is vital that all are accounted for in your final skill, and you should double check to be sure, adjust as necessary to ensure the principles are addressed.

Each skill should also contain an example of the skill being used. To do this, create a fake question by the user, and then use the very skill you have developed to generate a response to that question. The prompter will review this example and provide you with instructions on how to improve and tweak the skill as necessary. This skill itself is an example of how to create a skill.