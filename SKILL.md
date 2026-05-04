---
name: agents-md-clarifier
description: Use when the user wants to create, review, tighten, or refactor a global or local AGENTS.md so instructions act as explicit execution conditions rather than soft guidance. Covers SSOT declaration insertion, ambiguity audits, wording hardening, role and tool boundary clarification, and converting weak phrasing into operational rules.
---

# AGENTS.md Clarifier

## Overview

This skill audits `AGENTS.md` as an execution contract, not as explanatory prose.
Use it to tighten wording, remove ambiguity, clarify tool and role boundaries, and rewrite soft guidance into explicit operating conditions.

## Core Rule

Treat every `AGENTS.md` statement as an execution condition unless it is clearly explanatory context, an example, or an intentionally non-binding note.
If wording leaves easy escape hatches such as "consider", "prefer", "generally", or "if appropriate", assume the instruction is weaker than intended unless the surrounding context clearly demands discretionary language.

## SSOT Declaration Rule

When the target `AGENTS.md` does not already declare that it is the SSOT or an equivalent execution contract for its scope, add an appropriate declaration before tightening individual rules.

- Global target: use a global SSOT declaration when the file is `~/.codex/AGENTS.md`, is identified by the user as the global constitution, or explicitly governs all workspaces.
- Local target: use a scoped SSOT declaration for any workspace or subdirectory `AGENTS.md`.
- Do not duplicate an existing equivalent declaration. Treat wording such as "SSOT", "execution contract", "operational contract", "binding instructions", or "source of truth for this scope" as equivalent when it clearly binds Codex behavior.
- Insert the declaration at the start of the file. If the file has YAML frontmatter, insert it after the frontmatter. If the file begins with a single H1 title, insert it immediately after that title.
- Preserve priority order: the declaration must not claim authority over system instructions, developer instructions, explicit user requests, or more local `AGENTS.md` files.

Use these templates unless the existing document needs narrower wording:

Global:

```md
このファイルは `~/.codex/AGENTS.md` として機能するグローバル指示であり、Codex の共通実行条件を定義する SSOT である。
本書は助言集ではなく、既定動作、禁止事項、優先順位、ツール選択、検証条件を拘束する運用契約として扱う。
ワークスペースごとの `AGENTS.md` が存在する場合、本書は共通基盤として扱い、実際の運用ではより局所の `AGENTS.md` を優先する。
```

Local:

```md
このファイルは、このディレクトリ配下における Codex の局所 `AGENTS.md` であり、本スコープ内の実行条件を定義する SSOT である。
本書は助言集ではなく、既定動作、禁止事項、優先順位、ツール選択、検証条件を拘束する運用契約として扱う。
上位の `AGENTS.md`、システム指示、開発者指示、ユーザーの明示要求と競合する場合は、Codex の優先順位規則に従う。
より深い階層に別の `AGENTS.md` が存在する場合、そのスコープではより局所のファイルを優先する。
```

## When To Use

Use this skill when the user wants any of the following:

- Review an `AGENTS.md` for contradictions, weak wording, or hard-to-apply rules.
- Rewrite a global or local `AGENTS.md` so Codex behavior becomes more deterministic.
- Clarify tool roles such as `Computer Use`, Accessibility, `agent-browser`, subagents, or external memory.
- Convert policy text into operational text with explicit triggers and outcomes.
- Audit whether a skill file, wrapper script comment, or nearby instruction set aligns with `AGENTS.md`.

## Audit Workflow

1. Read the target `AGENTS.md` and determine whether it is global or local.
2. Check whether the target already has an SSOT or equivalent execution-contract declaration for its scope.
3. If the declaration is missing and the user asked for edits, add the appropriate declaration before other rewrites. If the user asked for review only, report the missing declaration as the first finding and include the declaration to add.
4. Isolate the sections that control behavior, tool choice, ordering, escalation, or delegation.
5. Identify wording that weakens execution, especially ambiguity, optionality, fuzzy thresholds, duplicated authority, or undefined trigger conditions.
6. Distinguish three categories:
   - True contradiction: two statements cannot both be followed.
   - Decision ambiguity: the intent is visible but the trigger or priority is under-specified.
   - Weak execution wording: the sentence sounds strict to a human but leaves enough discretion that the model may do nothing.
7. Rewrite only as much as needed to preserve the user's intended policy while making the action path explicit.
8. If the user asked for review only, report findings first with precise locations.
9. If the user asked for edits, patch the file directly and keep the rewritten language terse and operational.

## Wording Rules

Prefer verbs that directly constrain execution:

- Strong: `use`, `do not use`, `treat as`, `must`, `always`, `only when`, `unless`, `first`, `after`, `before`, `prefer X over Y`, `if A, do B`
- Weak: `consider`, `review`, `ideally`, `generally`, `basically`, `as needed`, `if appropriate`, `candidate`, `recommended`, `desirable`

When tightening wording:

- Replace weak modality with explicit triggers and outcomes.
- Replace vague sequencing with ordered steps.
- Replace overlapping responsibilities with named role boundaries.
- Replace "may" style language unless the flexibility is intentional and necessary.
- Keep exceptions explicit and narrow.

## What To Preserve

Do not rewrite for style alone.
Preserve the original:

- priority order
- safety constraints
- user intent
- local-over-global precedence
- domain-specific terminology

Only strengthen wording when it reduces interpretive drift without changing policy.

## Output Shape

When reviewing, output:

- the issue
- why it weakens execution
- the exact section or line reference
- a tighter rewrite

When editing, make the patch and summarize:

- whether an SSOT declaration was added or already present
- what behavior is now explicit
- what ambiguity was removed
- any remaining areas that are intentionally flexible

## Special Attention Areas

Pay extra attention to these recurrent failure patterns:

- `AGENTS.md` files that lack an SSOT or equivalent execution-contract declaration
- "soft" verbs that lead to non-action
- instructions that describe intent but not trigger conditions
- tool descriptions that do not define role boundaries
- sequencing rules that rely on implied order
- delegation text that says "first candidate" but does not actually require delegation
- policy text that sounds authoritative but still allows the model to default to doing nothing
