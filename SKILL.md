---
name: agents-md-clarifier
description: "Audit/edit global/local AGENTS.md execution contracts: SSOT declarations, weak wording, reasoning/verification gates, maintenance rules, tool/delegation boundaries, and nearby instruction alignment. Use for AGENTS.md creation, review, tightening, or refactor."
---

# AGENTS.md Clarifier

This `SKILL.md` is the local execution contract for this skill when the skill is selected.
Codex must treat this file's trigger assumptions, workflow, tool boundaries, file boundaries, and output shape as binding instructions within this skill's scope.
This file does not override system instructions, developer instructions, explicit user requests, applicable `AGENTS.md` files, or more specific local execution contracts.

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
- Clarify tool roles such as `Computer Use`, Accessibility, `agent-browser`, or subagents.
- Convert policy text into operational text with explicit triggers and outcomes.
- Audit whether a skill file, wrapper script comment, or nearby instruction set aligns with `AGENTS.md`.

## Target Resolution

Before auditing or editing, resolve exactly one target `AGENTS.md`.

Use this order:

1. If the user provides an explicit `AGENTS.md` path, use that path.
2. If the user provides a structured file reference with a `path`, use that path when it points to an `AGENTS.md`.
3. If the current working directory contains `AGENTS.md`, treat `./AGENTS.md` as the target.
4. If the current working directory is inside a workspace with one or more ancestor `AGENTS.md` files, use the nearest ancestor unless the user clearly asks for the global file.
5. If the user asks for the global constitution, use `~/.codex/AGENTS.md`.
6. If more than one target is plausible, stop and ask for the target path.

Do not audit multiple `AGENTS.md` files in one pass unless the user explicitly asks for cross-scope comparison. For cross-scope comparison, identify the local and global files separately and preserve the local-over-global priority order.

## Audit Workflow

1. Resolve exactly one target `AGENTS.md` using the Target Resolution rules.
2. Read the target `AGENTS.md` completely and determine whether it is global or local.
3. Check whether the target already has an SSOT or equivalent execution-contract declaration for its scope.
4. If the declaration is missing and the user asked for edits, add the appropriate declaration before other rewrites. If the user asked for review only, report the missing declaration as the first finding and include the declaration to add.
5. Isolate the sections that control behavior, tool choice, ordering, escalation, delegation, file ownership, or browser mode selection.
6. Identify wording that weakens execution, especially ambiguity, optionality, fuzzy thresholds, duplicated authority, undefined trigger conditions, or body-only rules that should be visible earlier in the file.
7. When auditing nearby instruction sets, skill text, wrapper script comments, or repo notes, compare them against the target `AGENTS.md` and classify each mismatch as one of:
   - subordinate alignment issue: the nearby text should be rewritten to follow `AGENTS.md`
   - migration candidate: the nearby text contains a durable execution rule that belongs in `AGENTS.md`
   - non-binding context: the nearby text can remain explanatory and must not be treated as an execution contract
8. Check whether high-risk decisions, debugging, review, source-of-truth edits, skill/plugin maintenance, hooks, MCP/app settings, external writes, deletion, and persistent data changes have an explicit reasoning and verification contract. The contract must separate premises, evidence, counterevidence or exception checks, unknowns, validation results, and stop conditions when those elements affect execution.
9. Check AGENTS.md maintenance gates:
   - If the target governs global or reusable Codex behavior, it must say that creating, revising, or materially updating an `AGENTS.md` requires `agents-md-clarifier` before final reporting, cache/runtime reflection, or commit.
   - If the target governs a narrower workspace and clearly inherits the global gate, accept delegation to the global gate; any local override must preserve the same completion condition.
   - Do not require a local `AGENTS.md` to duplicate the full global contract when it clearly inherits it and does not narrow or override it.
10. Distinguish three categories:
   - True contradiction: two statements cannot both be followed.
   - Decision ambiguity: the intent is visible but the trigger or priority is under-specified.
   - Weak execution wording: the sentence sounds strict to a human but leaves enough discretion that the model may do nothing.
11. Rewrite only as much as needed to preserve the user's intended policy while making the action path explicit.
12. If the user asked for review only, report findings first with precise locations.
13. If the user asked for edits, patch the file directly and keep the rewritten language terse and operational.
14. If the change makes `agents/openai.yaml` UI metadata stale, update `display_name`, `short_description`, or `default_prompt` without moving binding execution rules into that file.

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

## Boundary Checks

When the target controls role or tool selection, make the boundary executable:

- Define which tool, agent role, file, or source is primary for each trigger.
- Define when fallback is allowed and what failed or missing condition permits it.
- Define stop conditions for unresolved target paths, conflicting authorities, missing source-of-truth files, or cache-only files.
- Keep global rules in global `AGENTS.md` and local workflow rules in the most specific applicable `AGENTS.md` or implementation source.
- Do not move binding policy into README, comments, memories, or UI metadata unless `AGENTS.md` explicitly delegates that authority.

## Reasoning Contract Checks

When the target touches high-risk execution, make the reasoning path executable:

- Require Codex to separate premises, evidence, counterevidence or exception checks, unknowns, and validation results before finalizing the action.
- Treat numeric confidence, named prompting techniques, persona debate, and self-review as insufficient unless they are tied to concrete evidence and verification conditions.
- Require explicit stop conditions when evidence is missing, the source of truth cannot be resolved, validation cannot be run, or competing authorities conflict.
- Keep reusable global reasoning policy in global `AGENTS.md`; put only local refinements or stricter gates into local `AGENTS.md`.
- When auditing a skill, plugin, hook, or nearby workflow against `AGENTS.md`, flag missing reasoning-contract handling as a subordinate alignment issue unless the target is intentionally simple and has no high-risk judgment surface.

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
- whether `agents/openai.yaml` was updated
- any remaining areas that are intentionally flexible

## Special Attention Areas

Pay extra attention to these recurrent failure patterns:

- `AGENTS.md` files that lack an SSOT or equivalent execution-contract declaration
- "soft" verbs that lead to non-action
- instructions that describe intent but not trigger conditions
- reasoning requirements that do not separate premises, evidence, counterevidence, unknowns, validation, and stop conditions
- `AGENTS.md` maintenance rules that do not require `agents-md-clarifier` as an editing completion gate
- tool descriptions that do not define role boundaries
- sequencing rules that rely on implied order
- delegation text that says "first candidate" but does not actually require delegation
- policy text that sounds authoritative but still allows the model to default to doing nothing
