# Chapter 3: Claude Code × GitHub の実践ワークフロー

## Claude Code は Git を理解している

Claude Code はプロジェクトの Git 状態を**常に把握**しています。

- どのブランチにいるか
- どのファイルが変更されているか
- 最近のコミット履歴
- リモートとの差分

だから「コミットして」と頼むだけで、適切なコマンドを実行してくれます。

## 日常ワークフロー

Claude Code を使った典型的な作業の流れです。

### 1. 作業開始

```
最新の状態にして
```

Claude Code が `git pull` を実行し、リモートの最新変更を取り込みます。

### 2. 作業中

普通に Claude Code を使って作業します。ファイルの作成、編集、削除。

途中で何を変更したか確認したくなったら:

```
今の変更内容を見せて
```

Claude Code が `git diff` を実行し、変更内容を表示します。

### 3. 区切りのいいところでセーブ

```
ここまでの変更をコミットして
```

Claude Code が:
1. `git status` で変更を確認
2. `git add` で適切なファイルをステージング
3. `git commit -m "適切なメッセージ"` でコミット

を一括で行います。コミットメッセージも自動で考えてくれます。

### 4. GitHub にアップロード

```
push して
```

Claude Code が `git push` でリモートに送ります。

## 実践: ブランチで安全に作業する

### なぜブランチを使うか

main ブランチで直接作業すると:
- 壊れたコードがそのまま main に入る
- 「やっぱりやめたい」が難しい

ブランチを使うと:
- 失敗しても main は無傷
- 完成してからマージすればいい

### Claude Code でブランチを使う

```
「レポートテンプレート改善」用の新しいブランチを作って
```

Claude Code が `git checkout -b feature/improve-template` を実行。

作業が完了したら:

```
この変更でPRを作って
```

Claude Code が:
1. `git push` でブランチをリモートに送る
2. `gh pr create` でプルリクエストを作成

## 実践: Claude Code でコードレビュー

GitHub 上の PR を Claude Code にレビューしてもらうことも可能です。

```
PR #3 の内容を確認して、問題があれば教えて
```

Claude Code が `gh pr view 3` や `gh pr diff 3` を使って PR の内容を読み、フィードバックを返します。

## 実践: git worktree で並列作業する

### worktree とは

`git worktree` は **同じリポジトリの複数ブランチを別々のフォルダで同時に開ける** Git の機能です。

通常はブランチを切り替えるとき `git checkout` でカレントディレクトリの内容が丸ごと書き換わりますが、worktree を使うと：

- ブランチ A 用のフォルダ
- ブランチ B 用のフォルダ
- ブランチ C 用のフォルダ

を **同時に存在させたまま** 作業できます。

### Claude Code と相性が良い理由

Claude Code は「フォルダ単位で1セッション」起動するのが基本です。worktree と組み合わせると、こんなことができます。

1. **並列セッション**: メインのフォルダで機能開発を Claude Code に任せている間、別 worktree で別の Claude Code セッションを立ち上げて、バグ修正やレビューを並行で進められる
2. **実験の隔離**: 「Claude にこのリファクタを丸ごと任せたい」というとき、worktree に独立したコピーを作って試せる。失敗したら worktree ごと捨てればいい
3. **stash いらず**: 「途中だけど別のブランチを見たい」というときに `git stash` する必要がない

### 基本的な使い方

```bash
# 新しい worktree を作成（新しいブランチも同時に作る）
git worktree add ../myproject.wt/feat-login -b feat-login

# 作成した worktree に移動
cd ../myproject.wt/feat-login

# Claude Code を起動して作業
claude

# 作業終了後、worktree を削除
git worktree remove ../myproject.wt/feat-login
```

### Claude Code に頼む場合

コマンドを覚えていなくても、Claude Code に頼めば OK です。

```
worktree を作って feat-login ブランチで作業したい
```

```
今ある worktree 一覧を見せて
```

```
feat-login の worktree を削除して
```

### おすすめのフォルダ構成

worktree を散らかさないために、リポジトリと並べて専用フォルダを作るのがおすすめです。

```
~/Workspace/repos/myproject/           ← メインの worktree（main 追従）
~/Workspace/repos/myproject.wt/        ← worktree 置き場
  ├── feat-login/
  ├── fix-bug-123/
  └── experiment-refactor/
```

### 注意点

- **`node_modules` などは共有されない**: worktree ごとに `npm install` が必要です（`.gitignore` 対象のファイルは worktree 間でコピーされません）
- **`.env` ファイルも手動コピー**: 同じく `.gitignore` 対象なので、必要なら手動で持ってくる必要があります
- **同じブランチを2か所で開けない**: Git の制約で、すでに別の worktree で開いているブランチは別フォルダで開けません

### いつ使うか

最初は使わなくても困りません。以下のような状況になったら検討してみてください。

- 「機能開発の途中で緊急のバグ修正が必要になった」
- 「Claude Code に長時間タスクを任せたいけど、その間に別の作業もしたい」
- 「大きなリファクタを試したいけど、メインの作業環境は壊したくない」

## よくあるトラブルと対処法

### 「push が拒否された」

```
! [rejected]        main -> main (non-fast-forward)
```

**原因**: リモートに自分が持っていない変更がある。

**対処**: 先に `pull` してから `push` する。

```
pull してから push して
```

### 「コンフリクト（衝突）が起きた」

```
CONFLICT (content): Merge conflict in ファイル名
```

**原因**: 同じファイルの同じ部分が別々に変更された。

**対処**: Claude Code に解決を頼める。

```
コンフリクトを解消して。[こちらの変更/リモートの変更]を優先して
```

### 「間違えてコミットした」

**直前のコミットメッセージを直したい**:
```
直前のコミットメッセージを「〇〇」に修正して
```

**直前のコミット自体を取り消したい**:
```
直前のコミットを取り消して（変更は残して）
```

> **注意**: すでに `push` した変更を取り消すのは影響が大きいです。push 前なら安全に取り消せますが、push 後は慎重に。

## GitHub CLI のよく使うコマンド一覧

Claude Code に頼めばすべて代行してくれますが、知っておくと便利です。

### リポジトリ操作

| コマンド | 説明 |
|---------|------|
| `gh repo create` | リポジトリ作成 |
| `gh repo clone ユーザー/リポ` | クローン |
| `gh repo view` | リポジトリ情報を表示 |

### プルリクエスト

| コマンド | 説明 |
|---------|------|
| `gh pr create` | PR を作成 |
| `gh pr list` | PR 一覧 |
| `gh pr view 番号` | PR の詳細 |
| `gh pr merge 番号` | PR をマージ |
| `gh pr diff 番号` | PR の差分を表示 |

### Issue

| コマンド | 説明 |
|---------|------|
| `gh issue create` | Issue を作成 |
| `gh issue list` | Issue 一覧 |
| `gh issue view 番号` | Issue の詳細 |

## まとめ: Claude Code ユーザーが知っておくべきこと

### 概念として理解しておくこと

1. **Git = セーブポイント管理**: commit で変更を記録、log で履歴を確認
2. **GitHub = クラウド保存**: push で上げる、pull で取得する
3. **ブランチ = 安全な作業場**: main を壊さずに試行錯誤できる
4. **PR = 変更の提案**: レビューを経て main に取り込む

### コマンドは覚えなくていい

Claude Code に自然言語で頼めば、適切な git / gh コマンドを実行してくれます。

ただし **「何が起きているか」を理解しておく** ことで:
- Claude Code の操作結果を正しく判断できる
- トラブル時に的確な指示を出せる
- 「push していいか」「ブランチを切るべきか」の判断ができる

### 最低限のルール

1. **秘密情報は絶対にコミットしない**（.env、APIキーなど）
2. **push する前に確認する**（push は他の人にも影響する）
3. **迷ったら `git status`**（またはClaude Codeに「状態を見せて」）

## 演習

→ `exercises/03-claude-code-github/instructions.md` を開いて、演習に取り組みましょう
