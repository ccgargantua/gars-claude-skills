# Coding agents

Location: `agents/`, one flat file per agent (`agents/<name>.md`), mirroring how Claude Code actually loads them from `~/.claude/agents/` or a project's `.claude/agents/`. This is a different shape from `skills/`: an agent's frontmatter carries `name`, `description`, `tools`, and `model`, not just `name`/`description`, and there is no per-agent subfolder or references/ directory.

## What's here

Five agents that together form a permission-gated pipeline for writing and shipping code changes:

- **code-writer** - implements the change: reads, edits, writes files. Applies the `code-philosophy` skill before any non-trivial edit.
- **code-reviewer** - read-only review of a diff or existing files against the same `code-philosophy` rubric. No Edit/Write access.
- **code-tester** - read-only: runs the project's actual test command, diagnoses failures, and reasons about coverage gaps. No Edit/Write access.
- **code-auditor** - read-only security review, either of existing code or of a specific dependency someone wants to install. Renders an explicit APPROVE/REFUSE verdict. Has WebFetch/WebSearch to check CVEs and advisories.
- **dependency-installer** - the only agent allowed to run install commands. Requires all three of: exact package/version/ecosystem, a `code-auditor` verdict for that exact package, and explicit user approval, before it will touch a manifest.

## The pipeline

`code-writer`, `code-reviewer`, and `code-tester` never install anything themselves. If any of them decides a new dependency is needed, they stop and report the package name, ecosystem, version constraint, and why, rather than reaching for `npm install` or equivalent. That report routes to `code-auditor`, which renders APPROVE or REFUSE (a recommendation, not a hard block, the user has final say). Only once a dependency has both an auditor verdict and explicit user sign-off does `dependency-installer` act, and it treats those as two independent gates: an APPROVE is not itself approval to proceed.

## How this project keeps them current

Same discipline as `skills/`: `agents/` is a periodic snapshot pulled from the global `~/.claude/agents/`, never the other way around. The global files are read-only from this project's perspective. When resyncing, diff each project file against its global counterpart and port over any real content changes; if the global version has drifted from the user's own style rules (for example, picked up an em-dash), fix that on the way in rather than carrying the drift into this repo.

## Origin

Ported into this project in 2026-08-17 from the global agent set, with a Minimalism-only pass applied (em-dashes swapped for plain punctuation, wording otherwise untouched) rather than the full `skill-creation` structure, since these are agent definitions, not skills, and don't carry a Principles or Example section in Claude Code's own format.
