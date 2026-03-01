# Chapter 4: Skills - カスタムコマンドを作る

## Skillsとは？

Skillsは、Claude Codeに **カスタムのスラッシュコマンド** を追加する仕組みです。
よく使う操作をコマンド化して、再利用できるようにします。

## スラッシュコマンドの基本

Claude Codeには組み込みコマンドがあります:

| コマンド | 説明 |
|---------|------|
| `/help` | ヘルプ表示 |
| `/clear` | 会話クリア |
| `/compact` | 会話圧縮 |
| `/cost` | コスト表示 |

Skillsを使うと、これに **自分だけのコマンド** を追加できます。

## Skillの作り方

### ファイル配置

```
.claude/
└── commands/
    └── my-command.md      # /my-command で呼び出せる
```

### Skillファイルの書き方

`.claude/commands/report.md`:
```markdown
$ARGUMENTS を読み込み、以下の構造でレポートを生成してください。

## 出力フォーマット
1. タイトル
2. 一言サマリー（1文）
3. 背景と目的
4. 要点（箇条書き3〜5個）
5. 課題・限界
6. 次のアクション（あれば）

## ルール
- 専門用語には括弧で簡単な説明を添える
- 各セクションは3行以内に収める
```

→ `/report paper/sample.pdf` で呼び出せる

> **ポイント**: 「要約して」と自由に頼むのと違い、Skillは**出力フォーマットを固定**できます。
> 毎回同じ構造でレポートが出るので、再現性が高く、後工程（スライド化など）に繋げやすいのがSkill化する最大のメリットです。

### 引数の使い方

- `$ARGUMENTS` — コマンドに渡された引数全体が入る
- 例: `/my-command hello world` → `$ARGUMENTS` は `hello world`

## プロジェクト用 vs ユーザー用

| 配置場所 | スコープ | 例 |
|---------|--------|---|
| `.claude/commands/` | このプロジェクトだけ | プロジェクト固有の操作 |
| `~/.claude/commands/` | 全プロジェクト共通 | 個人でいつも使うもの |

## 実用的なSkill例

### コードレビュー
```markdown
# .claude/commands/review.md
以下のファイルをレビューしてください:
- バグの可能性がある箇所
- パフォーマンス改善の余地
- 可読性の改善点

対象: $ARGUMENTS
```

### コミットメッセージ生成
```markdown
# .claude/commands/commit-msg.md
現在のgit diffを確認し、Conventional Commits形式で
コミットメッセージを提案してください。
```

## 演習

→ `exercises/04-skills/instructions.md` を開いて、演習に取り組みましょう
