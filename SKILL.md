---
name: agents-md-clarifier
description: "Audit/edit one global or local AGENTS.md for interpretation drift (解釈ぶれ), explicit decision flow, and policy-preserving clarity; cover SSOT, authority, ownership, and maintenance. Use for AGENTS.md creation, review, clarification, or refactor."
---

# AGENTS.md Clarifier

This `SKILL.md` is the local execution contract for this skill when the skill is selected.
The host agent (Codex or Claude Code) must treat this file's trigger assumptions, workflow, tool boundaries, file boundaries, and output shape as binding instructions within this skill's scope.
This file does not override system instructions, developer instructions, explicit user requests, applicable `AGENTS.md` files, or more specific local execution contracts.

## Overview

This skill audits `AGENTS.md` as an execution contract, not as explanatory prose.
Use it to make conditions and action flow explicit while preserving authority, policy strength, and intentional discretion.

## Core Rule

Treat every `AGENTS.md` statement as an execution condition unless it is clearly explanatory context, an example, or an intentionally non-binding note.
Determine the required behavior from the user's declared meaning and authoritative context. Do not downgrade a user-required action to optional merely because it says "recommended", "should", "prefer", or "推奨". Express its applicability, required action, and permitted exceptions explicitly. Preserve optionality only when authoritative input actually grants it; unresolved intent is a finding, not permission to skip.
Treat `prefer X over Y` as an explicit default ordering only when the rule identifies the compared routes and preserves any governing exception or higher-priority condition.

## Shared Clarity Criteria

Before authoring or auditing, read [指示文の解釈を揃える基準](../skill-md-clarifier/references/instruction-clarity.md). This source resource is owned by the sibling standalone `skill-md-clarifier`; both clarifiers use the same maintained criteria. If that resource cannot be resolved, report the missing input and stop the affected clarity work instead of inventing a substitute.

Apply the criteria to AGENTS-specific scope, authority inheritance, mandatory/default/discretionary actions, exception reach, role handoffs, and completion boundaries. In edit mode, supply them as authoring inputs before constructing the candidate; in review mode, use them read-only. Keep the final semantic decision within Operation Modes And Ownership below.

## SSOT Declaration Rule

When the target `AGENTS.md` does not already declare that it is the SSOT or an equivalent execution contract for its scope, add an appropriate declaration before tightening individual rules.

- Global target: use a global SSOT declaration when the file is the host's global rules file (Codex: `~/.codex/AGENTS.md`; Claude Code: `~/.claude/AGENTS.md`), is identified by the user as the global constitution, or explicitly governs all workspaces.
- Local target: use a scoped SSOT declaration for any workspace or subdirectory `AGENTS.md`.
- Do not duplicate an existing equivalent declaration. Treat wording such as "SSOT", "execution contract", "operational contract", "binding instructions", or "source of truth for this scope" as equivalent when it clearly binds the host agent's behavior.
- Insert the declaration at the start of the file. If the file has YAML frontmatter, insert it after the frontmatter. If the file begins with a single H1 title, insert it immediately after that title.
- Preserve the complete applicable priority order. A more-local ordinary rule may specialize an ordinary higher-scope rule, but it must not weaken a higher-priority named invariant or another rule that the applicable authority order places above it.

Use these templates unless the existing document needs narrower wording:

Global (replace `<global-path>` and `<agent>` with the target's host: `~/.codex/AGENTS.md` and Codex, or `~/.claude/AGENTS.md` and Claude Code):

```md
このファイルは `<global-path>` として機能するグローバル指示であり、<agent> の共通実行条件を定義する SSOT である。
本書は助言集ではなく、既定動作、禁止事項、優先順位、ツール選択、検証条件を拘束する運用契約として扱う。
より局所の `AGENTS.md` はスコープ内の通常ルールを具体化できるが、上位の名前付き不変条件を弱めず、完全な優先順位規則に従う。
```

Local (`<agent>` is the agent that reads the file: Codex, Claude Code, or both as "Codex と Claude Code"):

```md
このファイルは、このディレクトリ配下における <agent> の局所 `AGENTS.md` であり、本スコープ内の実行条件を定義する SSOT である。
本書は助言集ではなく、既定動作、禁止事項、優先順位、ツール選択、検証条件を拘束する運用契約として扱う。
上位の `AGENTS.md`、システム指示、開発者指示、ユーザーの明示要求と競合する場合は、<agent> の優先順位規則に従う。
より深い階層の `AGENTS.md` は、そのスコープ内の通常ルールを具体化できるが、上位の名前付き不変条件を弱めない。
```

## When To Use

Use this skill when the user wants any of the following:

- Review an `AGENTS.md` for contradictions, weak wording, or hard-to-apply rules.
- Rewrite a global or local `AGENTS.md` so the host agent's behavior becomes more deterministic.
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
5. If the user asks for the global constitution, use the running host's global rules file: in Codex, `~/.codex/AGENTS.md`; in Claude Code, `~/.claude/AGENTS.md`. Edit the other host's global file only when the user explicitly names that host or path.
6. If more than one target is plausible, stop and ask for the target path.

Applicable higher-scope `AGENTS.md` files are authority inputs, not additional audit targets. Read only the higher-scope text needed to interpret the target and its governing priority order.

Do not audit multiple target `AGENTS.md` files in one pass unless the user explicitly asks for cross-scope comparison. For cross-scope comparison, identify each target separately and preserve the complete applicable authority order rather than assuming blanket local-over-global precedence.

## Operation Modes And Ownership

Select one operation before production:

- Review mode is read-only. It returns findings and tighter replacement wording without editing the target.
- Edit mode authors one complete first-pass candidate that integrates every known requirement before final clarification.

In edit mode, finish construction before starting the final clarifier decision. Starting that decision seals the candidate and makes the clarifier phase read-only: it may confirm the final bytes or return a concrete failed finding, but it must not patch, finish, or reinterpret the candidate while acting as checker.

If that admitted final decision fails, end the checker phase and apply the applicable cause-bound recovery rule to the smallest affected authoring scope. The checker does not become the repair producer. The applicable `AGENTS.md` owns the number, independence, order, and rerun conditions of required checks; this skill does not add a generic second review or replace an exact acceptance bundle.

## Audit Workflow

1. Resolve exactly one target `AGENTS.md` using the Target Resolution rules.
2. Read the target `AGENTS.md` completely, determine whether it is global or local, and read only the applicable higher-scope authority needed to interpret it.
3. Check whether the target already has an SSOT or equivalent execution-contract declaration for its scope.
4. If the declaration is missing and the user asked for edits, add the appropriate declaration before other rewrites. If the user asked for review only, report the missing declaration as the first finding and include the declaration to add.
5. Isolate the sections that control behavior, tool choice, ordering, escalation, delegation, file ownership, or browser mode selection.
6. Apply Shared Clarity Criteria to behavior-controlling rules. Trace relevant conditions through the responsible actor's action, exceptions, handoff, and end state; identify concrete missing relations or action-changing interpretations rather than optional wording by itself.
7. When auditing nearby instruction sets, skill text, wrapper script comments, or repo notes, compare them against the target `AGENTS.md` and classify each mismatch as one of:
   - subordinate alignment issue: the nearby text should be rewritten to follow `AGENTS.md`
   - migration candidate: the nearby text contains a durable execution rule that belongs in `AGENTS.md`
   - non-binding context: the nearby text can remain explanatory and must not be treated as an execution contract
8. When the target governs high-risk decisions, debugging, source-of-truth edits, hooks, MCP/app settings, external writes, deletion, or persistent data, check whether it correctly inherits or locally specializes the applicable named reasoning and verification invariants. Flag a conflict, weakening, or unnecessary duplicate; do not require a local copy of a higher-scope kernel.
9. Check AGENTS.md maintenance gates:
   - Identify which applicable `AGENTS.md` owns the maintenance decision before proposing text.
   - If the target is that owner, confirm its current maintenance rule has an executable completion condition.
   - If a narrower target clearly inherits the owning rule, accept that inheritance; flag only a conflicting or weakening local override.
   - Do not insert a duplicate maintenance gate solely because this skill describes the audit route.
10. Distinguish four categories:
   - True contradiction: two statements cannot both be followed.
   - Decision ambiguity: the intent is visible but the trigger or priority is under-specified.
   - Weak execution wording: an authoritative requirement is established, but the sentence leaves its required trigger or action optional, including a required "推奨" that can be misread as skippable. Explicitly authorized permission or discretion is not this defect.
   - Producer-quality / verification-topology defect: the contract defers semantic quality to a later layer, launders or duplicates a binding check, or adds a same-decision post-success gate.
11. Rewrite only as much as needed to preserve the user's intended policy while making the action path explicit.
12. If the user asked for review only, report findings first with precise locations.
13. If the user asked for edits, patch the file directly and keep the rewritten language terse and operational.
14. If the change makes `agents/openai.yaml` UI metadata stale, update `display_name`, `short_description`, or `default_prompt` without moving binding execution rules into that file.

## Wording Rules

Use the shared criteria's meaning-preservation boundary when changing modality, order, references, or examples. In an AGENTS.md, name the governing scope or rule when inheritance or an exception otherwise requires guesswork. Clarify only the relationships needed to determine action; do not duplicate the inherited rule or convert permitted judgment into a mandatory procedure.

## Boundary Checks

When the target controls role or tool selection, make the boundary executable:

- Define which tool, agent role, file, or source is primary for each trigger.
- Define when fallback is allowed and what failed or missing condition permits it.
- Define stop conditions for unresolved target paths, conflicting authorities, missing source-of-truth files, or cache-only files.
- Keep global rules in global `AGENTS.md` and local workflow rules in the most specific applicable `AGENTS.md` or implementation source.
- Do not move binding policy into README, comments, memories, or UI metadata unless `AGENTS.md` explicitly delegates that authority.

## Applicable-Kernel Alignment

The applicable `AGENTS.md` hierarchy owns cross-cutting reasoning, producer, acceptance, success, and recovery rules. This skill owns only the AGENTS-specific mechanics for locating those rules and testing the target against them; it does not restate or supply a second execution kernel.

When the target contains rules in those domains:

- map each rule to its owning higher rule or named invariant when one exists
- verify that local text specializes only its declared scope and preserves the owning rule's operational effect
- trace target-declared producer, checker, coordinator, and handoff roles to find overlapping ownership or a downstream layer that is expected to finish known semantics
- flag duplicated same-decision checks, unsupported post-success gates, or a local bypass only by reference to the applicable owning contract
- do not insert a canned reasoning checklist, generic acceptance bundle, or recovery policy when the target already inherits the decision

If the target itself owns a cross-cutting rule, audit whether its own triggers, owners, evidence, and stop conditions are executable. Base any rewrite on the user's current policy and applicable higher authority instead of importing a reusable kernel from this skill.

## What To Preserve

Do not rewrite for style alone.
Preserve the original:

- priority order
- safety constraints
- user intent
- the complete applicable authority order, including higher named invariants and lawful local specialization
- domain-specific terminology

Clarify wording within the user's declared policy, making intended obligations explicit even when the old words sound advisory. If clarification would require choosing an unresolved policy, report that decision instead of weakening its binding force or inventing another obligation.

## Output Shape

When reviewing, output:

- the issue
- why it weakens execution
- the exact section or line reference
- a source-grounded rewrite, or the unresolved policy decision under the shared criteria's output branch

For an interpretation finding, include the concrete condition, the supported competing actions or missing relation, and the policy boundary the rewrite preserves. Use existing issue, rationale, location, and rewrite fields; do not invent a separate result format. When the policy is unresolved, put that unresolved decision in `tighterRewrite` rather than filling it with invented conditions. Distinguish document-based evidence from any actual model execution evidence.

When editing, make the patch and summarize:

- which operation mode was used and where construction ended before final clarification
- whether an SSOT declaration was added or already present
- what behavior is now explicit
- what ambiguity was removed
- whether `agents/openai.yaml` was updated
- any remaining areas that are intentionally flexible

## Special Attention Areas

Pay extra attention to these recurrent failure patterns:

- `AGENTS.md` files that lack an SSOT or equivalent execution-contract declaration
- an established obligation whose trigger or action is left optional
- instructions that describe intent but not trigger conditions
- local reasoning or verification text that conflicts with, weakens, or needlessly duplicates its applicable owning contract
- `AGENTS.md` maintenance rules that conflict with or weaken the applicable owning maintenance invariant
- tool descriptions that do not define role boundaries
- sequencing rules that rely on implied order
- producer/checker text that does not identify the construction-to-final-decision boundary
- delegation text that promises a handoff without defining ownership or the completed output
- policy text that sounds authoritative but still allows the model to default to doing nothing
