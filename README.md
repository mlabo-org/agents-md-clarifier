# agents-md-clarifier

`agents-md-clarifier` audits or edits global and local `AGENTS.md` files as
execution contracts rather than loose guidance.

## What it audits

- scoped SSOT declarations and authority order;
- weak, ambiguous, duplicated, or contradictory instructions;
- producer ownership and single-bundle verification topology;
- reasoning, maintenance, tool, delegation, source, and runtime boundaries;
- alignment between `AGENTS.md` and nearby skills, wrappers, or instructions.

The skill preserves user intent and local-over-global precedence. It does not
turn optional style preferences into new policy or use a clarifier pass as a
substitute for first-pass semantic ownership.

## Installation

Clone the public repository into the Codex standalone-skill directory:

```bash
git clone https://github.com/mlabo-org/agents-md-clarifier.git ~/.codex/skills/agents-md-clarifier
```

Restart Codex or begin a new task after installation. For source development,
edit the Git checkout and treat installed/runtime copies as derived artifacts.

## Usage

```text
Use $agents-md-clarifier to audit this AGENTS.md and tighten its execution contract.
```

The skill resolves one target `AGENTS.md`, reads it completely, classifies
contract defects, applies only the requested edit or review scope, and reports
the resulting source and validation boundary.

## Repository layout

```text
.
├── SKILL.md
├── agents/
│   └── openai.yaml
├── LICENSE
└── README.md
```

## License

MIT
