# agents-md-clarifier

[日本語](#日本語) | [English](#english)

## 日本語

`agents-md-clarifier` は、グローバルまたはローカルの `AGENTS.md` を、説明文ではなく実行契約として監査・編集する、Codex と Claude Code の両方で使えるスキルです。目的は、読み手の推論深度が違っても、同じ事実と権限から同じ必須行動、禁止、許容範囲、停止判断、要求結果、完了条件を読み取れる指示にすることです。

これはモデル間の実行結果を実測したり、決定論的な一致を保証したりする仕組みではありません。指示に必要な条件と関係を明示し、読み手が常識や推測で補う余地を減らします。実際のモデル比較や追加評価は、このスキルから自動的には発生しません。

### 何を確認するか

- 対象スコープの SSOT 宣言と、適用される権限の優先順位
- 弱い、曖昧、重複、または矛盾した実行指示
- 適用条件、担当、入力、行動、順序、受け渡し、分岐、例外、停止、完了の関係
- 作成者の責任と、既存の最終監査における一つの意味上の受入判断
- reasoning、maintenance、tool、delegation、source、runtime の境界
- `AGENTS.md` と周辺の skill、wrapper、instruction の整合性

### 拘束力と「推奨」の扱い

明確化によって指示の拘束力を弱めません。読みやすさ、短文化、低推論への配慮を理由に、実行義務を任意へ変えたり、例外・裁量・省略の範囲を広げたりする候補は受け入れません。

単語だけで義務や任意性を決めません。ユーザー要求や適用される正本が実行を求めている「推奨」「そうするべき」「prefer」は、適用条件を満たしたら実行する指示として具体化します。方法の選択が許されていても、不実行の許可へ読み替えません。任意性を残せるのは、権限のある入力が選択や省略を明示している場合だけです。意味を決める根拠が不足している場合は、勝手に条件を足さず、未確定の判断として返します。

### 作成・監査の流れ

編集では、対象の正本、ユーザー要求、関係する既知の条件を作成前の入力として揃えます。その入力から、完全な候補を一度で構成します。既存の最終監査は、候補を補完・再構成する工程ではなく、拘束力と意味の境界が保たれているかを確認する工程です。最終監査のために既知の要件を後から追加したり、通常の修正ループを増やしたりしません。

レビューは読み取り専用です。編集モードは、指定された対象の変更だけを行います。対象は一つに確定し、曖昧な対象、競合する権威、見つからない正本、または必要な共有基準がない場合は、推測で進めず停止します。

正本やユーザー要求から判断が確定している場合は、その根拠を示して条件と行動の関係を明確にします。条件、例外、続行可否などの方針が正本にない場合は、監査者が新しい方針を作らず、不足する決定と分かれる行動を返します。

### 共有基準への依存

このスキルは、兄弟スキル `skill-md-clarifier` が管理する [指示文の解釈を揃える基準](https://github.com/mlabo-org/skill-md-clarifier/blob/main/references/instruction-clarity.md) を `../skill-md-clarifier/references/instruction-clarity.md` から読みます。基準本文をこの README に複製していません。共有基準を解決できない場合、該当する明確化監査を停止します。

二つの standalone skill は、同じユーザー skill root に兄弟ディレクトリとして配置してください。Codex は `~/.agents/skills` を直接読み、Claude Code では `~/.claude/skills` に各 checkout へのシンボリックリンクを置きます。

```text
<user-skill-directory>/
├── agents-md-clarifier/
└── skill-md-clarifier/
    └── references/instruction-clarity.md
```

通常の例として `~/.agents/skills` に clone する場合:

```bash
git clone https://github.com/mlabo-org/agents-md-clarifier.git ~/.agents/skills/agents-md-clarifier
git clone https://github.com/mlabo-org/skill-md-clarifier.git ~/.agents/skills/skill-md-clarifier
# Claude Code でも使う場合
mkdir -p ~/.claude/skills
ln -s ~/.agents/skills/agents-md-clarifier ~/.claude/skills/agents-md-clarifier
ln -s ~/.agents/skills/skill-md-clarifier ~/.claude/skills/skill-md-clarifier
```

別の skill root を使う場合も、二つの repository を同じ親ディレクトリに clone してください。インストール後は Codex または Claude Code を再起動するか、新しいタスク（セッション）を開始してください。

### インストール後の使用例

Codex では `$agents-md-clarifier`、Claude Code では `/agents-md-clarifier` で呼び出します。

```text
Use $agents-md-clarifier to audit this AGENTS.md and preserve its policy strength while making the decision flow explicit.
```

このスキルは対象の `AGENTS.md` を一つに確定して全文を読み、実行契約上の欠陥を分類し、依頼されたレビューまたは編集の範囲だけを適用して、変更された正本と検証境界を報告します。実行契約そのものは `SKILL.md` と共有基準が定めます。この README は利用説明です。

### リポジトリ構成

```text
.
├── SKILL.md
├── agents/
│   └── openai.yaml
├── local-capability-manifest.json
├── schemas/
│   └── local-capability/
│       ├── audit-edit-agents-md-input.schema.json
│       └── audit-edit-agents-md-output.schema.json
├── LICENSE
└── README.md
```

### ライセンス

MIT

## English

`agents-md-clarifier` is a skill for Codex and Claude Code for auditing or editing global and local `AGENTS.md` files as execution contracts rather than explanatory prose. Its purpose is to make readers with different reasoning depths derive the same required actions, prohibitions, permitted range, stop decisions, requested result, and completion condition from the same facts and authority.

This does not measure model behavior or guarantee deterministic agreement between models. It makes the necessary conditions and relationships explicit so that readers have less need to fill gaps with assumptions. Model comparisons and extra evaluation stages do not arise automatically from this skill.

### What it checks

- The scoped SSOT declaration and applicable authority order
- Weak, ambiguous, duplicated, or contradictory execution instructions
- The relationships among scope, actor, inputs, actions, ordering, handoffs, branches, exceptions, stops, and completion
- Producer ownership and one semantic acceptance decision in the existing final audit
- Reasoning, maintenance, tool, delegation, source, and runtime boundaries
- Alignment between `AGENTS.md` and nearby skills, wrappers, or instructions

### Preserving force, including “recommended” wording

Clarification must never weaken an instruction. A candidate is rejected if readability, shortening, or accommodation for lower reasoning turns a required action into an option or expands exceptions, discretion, or omission.

Modality is determined from the user request and applicable authoritative source, not from one word in isolation. When an authoritative request gives execution force to “recommended,” “should,” or “prefer,” the instruction is made concrete as: perform it when its applicability condition holds. Permission to choose a method does not become permission to skip the action. Optionality remains only when an authorized input explicitly grants a choice or omission. If the source does not settle the policy, the skill reports the unresolved decision and does not invent conditions.

### Authoring and audit flow

In edit mode, the authoritative target, user request, and all known relevant conditions are supplied as inputs before construction. One complete candidate is then authored from those inputs. The existing final audit confirms that the candidate preserves the force and meaning boundary; it does not complete, reinterpret, or routinely rewrite the candidate. Known requirements are not deferred until the final audit, and an extra repair loop is not added as a default.

Review mode is read-only. Edit mode changes only the requested target. The skill resolves exactly one target and stops rather than guessing when the target is ambiguous, authorities conflict, the source of truth is unavailable, or the required shared criteria cannot be resolved.

When the authoritative source or user request settles a decision, the result identifies that basis and makes the condition-to-action relationship explicit. When the source leaves a policy decision such as a condition, exception, or continuation rule unsettled, the skill reports the missing decision and the actions it would affect instead of creating a new policy.

### Shared criteria dependency

This skill reads the [Shared Instruction Clarity Criteria](https://github.com/mlabo-org/skill-md-clarifier/blob/main/references/instruction-clarity.md), maintained by the sibling skill `skill-md-clarifier`, at `../skill-md-clarifier/references/instruction-clarity.md`. The criteria are not duplicated in this README. If the shared criteria cannot be resolved, the affected clarity audit stops.

Place both standalone skills as sibling directories in the same user skill root. Codex reads `~/.agents/skills` directly; for Claude Code, add symlinks to each checkout under `~/.claude/skills`:

```text
<user-skill-directory>/
├── agents-md-clarifier/
└── skill-md-clarifier/
    └── references/instruction-clarity.md
```

For the usual `~/.agents/skills` example:

```bash
git clone https://github.com/mlabo-org/agents-md-clarifier.git ~/.agents/skills/agents-md-clarifier
git clone https://github.com/mlabo-org/skill-md-clarifier.git ~/.agents/skills/skill-md-clarifier
# Also for Claude Code
mkdir -p ~/.claude/skills
ln -s ~/.agents/skills/agents-md-clarifier ~/.claude/skills/agents-md-clarifier
ln -s ~/.agents/skills/skill-md-clarifier ~/.claude/skills/skill-md-clarifier
```

If you use another skill root, clone both repositories under the same parent directory. Restart Codex or Claude Code, or start a new task or session, after installation.

### Usage after installation

Invoke it as `$agents-md-clarifier` in Codex or `/agents-md-clarifier` in Claude Code.

```text
Use $agents-md-clarifier to audit this AGENTS.md and preserve its policy strength while making the decision flow explicit.
```

The skill resolves exactly one `AGENTS.md` target, reads it completely, classifies execution-contract defects, applies only the requested review or edit scope, and reports the changed source and validation boundary. The execution contract itself is defined by `SKILL.md` and the shared criteria. This README explains how to use the skill.

### Repository layout

```text
.
├── SKILL.md
├── agents/
│   └── openai.yaml
├── local-capability-manifest.json
├── schemas/
│   └── local-capability/
│       ├── audit-edit-agents-md-input.schema.json
│       └── audit-edit-agents-md-output.schema.json
├── LICENSE
└── README.md
```

### License

MIT
