# Chapter 1: 復習とセットアップ

## Vol.1 の振り返り

まず、Vol.1 で学んだことを簡単に振り返ります。

### Claude Code の基本ツール

| ツール | 役割 | 例 |
|-------|------|---|
| Read | ファイルを読む | 「README.mdを読んで」 |
| Edit | ファイルの一部を修正する | 「3行目を変更して」 |
| Write | 新しいファイルを作成する | 「hello.txtを作って」 |
| Bash | コマンドを実行する | 「git statusを実行して」 |
| Glob | ファイルを検索する | 「.mdファイルを一覧して」 |
| Grep | ファイルの中身を検索する | 「TODOを含むファイルを探して」 |

### 設定の仕組み

| 仕組み | 役割 | 配置場所 |
|-------|------|---------|
| CLAUDE.md | プロジェクトルールの定義 | プロジェクトルート |
| Skills | カスタムコマンド | `.claude/commands/` |
| settings.json | 権限・MCP等の設定 | `.claude/` |

### Subagents

Claude Codeは複雑なタスクを受けると、自動的に **Subagent**（子エージェント）を生成して並列処理します。Vol.1 では「Claude Codeが勝手にやってくれる」程度の理解でしたが、Vol.2 ではこれをより意識的に活用します。

## Vol.2 で新しく学ぶこと

| 機能 | 概要 | 学ぶ章 |
|------|------|-------|
| WebSearch / WebFetch | Webから最新情報を取得する | Ch.2 |
| Hooks | ツール実行の前後に自動処理を走らせる | Ch.3 |
| パイプラインSkills | 複数のSkillを繋げて多段処理する | Ch.4 |
| Agent Teams | 複数エージェントの役割分担と協調 | Ch.5 |

## このリポジトリの構成を確認

Vol.2 のリポジトリには、いくつかのSkillが最初から配置されています。確認してみましょう。

```
このリポジトリの .claude/commands/ の中身を見せて
```

4つのSkillファイルが見つかるはずです:

| Skill | コマンド | 役割 |
|-------|---------|------|
| research.md | `/research` | Web検索で情報を集める |
| analyze.md | `/analyze` | 収集した情報を分析する |
| report.md | `/report` | レポートを生成する |
| review.md | `/review` | 成果物にフィードバックする |

> **ポイント**: `/research` → `/analyze` → `/report` → `/review` の順に使うと、調査から完成品まで一貫した流れで作業できます。これが Vol.2 のメインフローです。

## 演習

→ `exercises/01-recap/instructions.md` を開いて、演習に取り組みましょう
