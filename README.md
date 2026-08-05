# agents-md-clarifier

[日本語](#日本語) | [English](#english)

## 日本語

`agents-md-clarifier` は、グローバルまたはローカルの `AGENTS.md` を、単なる説明文ではなく実行契約として監査・編集する Codex スキルです。

### 監査対象

- スコープに応じた SSOT 宣言と権限の優先順位
- 弱い、曖昧、重複、または矛盾した指示
- producer ownership と単一の semantic acceptance bundle による検証トポロジー
- reasoning、maintenance、tool、delegation、source、runtime の境界
- `AGENTS.md` と周辺の skill、wrapper、instruction との整合性

このスキルは、ユーザーの意図と local-over-global の優先順位を維持します。任意のスタイル上の好みを新しいポリシーに変えたり、clarifier の監査を first-pass semantic ownership の代わりに使ったりしません。

### 現在のソース、実行コピー、GitHub の関係

このスキルの保守環境では、次の三者を同じスキル内容として管理します。

- 権威ソース兼 Git working tree: `/Users/suzukimakoto/skills/agents-md-clarifier/`
- Codex が使用する派生 runtime copy: `/Users/suzukimakoto/.codex/skills/agents-md-clarifier/`
- 同じ Git 履歴の公開 remote: [mlabo-org/agents-md-clarifier](https://github.com/mlabo-org/agents-md-clarifier)

編集は権威ソースだけで行い、runtime copy を直接編集しません。変更はソース repository に commit し、その clean commit から runtime copy を更新し、同じ commit を GitHub の `origin/main` に push します。これにより、ローカルで実行されるスキルと GitHub で公開されるスキルを、同じ commit 内容に保ちます。GitHub 用の別 checkout や別の正本は作りません。

### 公開版のインストール

通常の standalone skill として直接インストールする場合は、公開 repository を Codex の skill directory に clone します。

```bash
git clone https://github.com/mlabo-org/agents-md-clarifier.git ~/.codex/skills/agents-md-clarifier
```

インストール後は、Codex を再起動するか新しいタスクを開始してください。

### 使用例

```text
Use $agents-md-clarifier to audit this AGENTS.md and tighten its execution contract.
```

このスキルは対象となる `AGENTS.md` を1つに確定して全文を読み、契約上の欠陥を分類し、依頼された編集またはレビューの範囲だけを適用して、変更されたソースと検証境界を報告します。

### リポジトリ構成

```text
.
├── SKILL.md
├── agents/
│   └── openai.yaml
├── LICENSE
└── README.md
```

### ライセンス

MIT

## English

`agents-md-clarifier` is a Codex skill that audits or edits global and local `AGENTS.md` files as execution contracts rather than loose guidance.

### What it audits

- Scoped SSOT declarations and authority order
- Weak, ambiguous, duplicated, or contradictory instructions
- Producer ownership and verification topology based on one semantic acceptance bundle
- Reasoning, maintenance, tool, delegation, source, and runtime boundaries
- Alignment between `AGENTS.md` and nearby skills, wrappers, or instructions

The skill preserves user intent and local-over-global precedence. It does not turn optional style preferences into new policy or use a clarifier audit as a substitute for first-pass semantic ownership.

### Current source, runtime copy, and GitHub relationship

In the maintainer environment, the following three locations represent the same skill content:

- Authoritative source and Git working tree: `/Users/suzukimakoto/skills/agents-md-clarifier/`
- Derived runtime copy used by Codex: `/Users/suzukimakoto/.codex/skills/agents-md-clarifier/`
- Public remote for the same Git history: [mlabo-org/agents-md-clarifier](https://github.com/mlabo-org/agents-md-clarifier)

Edits are made only in the authoritative source; the runtime copy is never edited directly. A change is committed in the source repository, the runtime copy is refreshed from that clean commit, and the same commit is pushed to GitHub at `origin/main`. This keeps the locally executed skill and the skill published on GitHub equivalent to the same commit. No separate GitHub checkout or second source of truth is used.

### Public installation

To install the public version directly as a standalone skill, clone the repository into the Codex skill directory:

```bash
git clone https://github.com/mlabo-org/agents-md-clarifier.git ~/.codex/skills/agents-md-clarifier
```

Restart Codex or begin a new task after installation.

### Usage

```text
Use $agents-md-clarifier to audit this AGENTS.md and tighten its execution contract.
```

The skill resolves exactly one target `AGENTS.md`, reads it completely, classifies contract defects, applies only the requested edit or review scope, and reports the changed source and validation boundary.

### Repository layout

```text
.
├── SKILL.md
├── agents/
│   └── openai.yaml
├── LICENSE
└── README.md
```

### License

MIT
