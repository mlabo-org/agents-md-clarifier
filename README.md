# agents-md-clarifier

`agents-md-clarifier` is a Codex skill for tightening `AGENTS.md` files so they behave like execution contracts instead of loose guidance.

It focuses on the failure mode many Codex users hit sooner or later:

> "I wrote the rule in AGENTS.md. Why does Codex still ignore it, reinterpret it, or behave inconsistently?"

This skill audits and rewrites `AGENTS.md` with explicit triggers, priorities, tool boundaries, and completion conditions.

## What It Does

- Adds a scoped SSOT / execution-contract declaration when missing.
- Finds weak wording such as "consider", "generally", "as needed", or "if appropriate".
- Converts vague guidance into explicit operating conditions.
- Clarifies global vs local `AGENTS.md` precedence.
- Tightens tool-role boundaries, including browser tools, Computer Use, subagents, and external memory.
- Preserves user intent while reducing interpretive drift.

## Before And After

Before:

```md
# AGENTS.md

Please prefer using the browser when appropriate.
Generally avoid editing unrelated files.
Review changes before finishing.
```

After:

```md
# AGENTS.md

This file is the local AGENTS.md for this repository and defines the execution contract for Codex within this scope.

## Browser Work

- If a task requires opening, inspecting, clicking, typing, or screenshotting a web page, use the Browser tool first.
- If the Browser tool is unavailable, report the unavailable condition and ask before switching to another browser route.

## File Edits

- Edit only files required by the current request.
- Do not modify unrelated files unless the user explicitly expands the scope.

## Completion

- After file edits, run `git status --short` before the final response.
- In the final response, summarize changed files and verification results.
```

## Repository Layout

```text
.
├── SKILL.md
├── agents/
│   └── openai.yaml
├── LICENSE
└── README.md
```

## Installation

Clone this repository into your Codex skills directory:

```bash
git clone https://github.com/mlabo-org/agents-md-clarifier.git ~/.codex/skills/agents-md-clarifier
```

Then restart Codex or open a new session so the skill list is refreshed.

## Usage

Ask Codex to use the skill against a target `AGENTS.md`:

```text
Use $agents-md-clarifier to review this AGENTS.md and tighten weak execution wording.
```

For direct editing:

```text
Use $agents-md-clarifier to add a missing SSOT declaration and rewrite this AGENTS.md as an execution contract.
```

## Core Idea

Longer instructions are not automatically stronger instructions.

An effective `AGENTS.md` needs:

- a clear scope
- an explicit authority declaration
- concrete trigger conditions
- narrow exceptions
- deterministic ordering
- observable completion conditions

Without those, Codex can treat rules as soft preferences, especially when the wording leaves escape hatches.

## License

MIT
