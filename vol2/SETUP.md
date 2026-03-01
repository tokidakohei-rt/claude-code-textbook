# セットアップガイド（Vol.2）

Vol.2 を始めるための準備ガイドです。Vol.1 を修了済みの方は、ほとんどの環境が揃っているはずです。

---

## 目次

1. [前提条件の確認](#1-前提条件の確認)
2. [このリポジトリの取り込み](#2-このリポジトリの取り込み)
3. [動作確認](#3-動作確認)
4. [Vol.1 未受講の方へ](#4-vol1-未受講の方へ)

---

## 1. 前提条件の確認

以下がインストール・設定済みであることを確認してください。

| 項目 | 確認コマンド | 期待される結果 |
|------|------------|--------------|
| Node.js v18+ | `node --version` | v18.x.x 以上 |
| Claude Code | `claude --version` | バージョンが表示される |
| Git | `git --version` | バージョンが表示される |

```bash
# まとめて確認
node --version && claude --version && git --version
```

すべてバージョンが表示されればOKです。

> **未インストールの場合**: Vol.1 の [SETUP.md](https://github.com/tokidakohei-rt/claude-code-textbook/blob/main/SETUP.md) の手順に従ってインストールしてください。

---

## 2. このリポジトリの取り込み

### ターミナルから

```bash
cd ~
git clone https://github.com/tokidakohei-rt/claude-code-textbook-vol2.git
cd claude-code-textbook-vol2
```

### VS Code / Cursor から

1. `Cmd+Shift+P`（Windows: `Ctrl+Shift+P`）でコマンドパレットを開く
2. 「Git: Clone」と入力して選択
3. リポジトリURL を入力:
   ```
   https://github.com/tokidakohei-rt/claude-code-textbook-vol2.git
   ```
4. 保存先フォルダを選択 → 「Open」

---

## 3. 動作確認

### 1) フォルダ構成の確認

```
claude-code-textbook-vol2/
├── CLAUDE.md           ← 講師AI用の指示ファイル
├── SETUP.md            ← このファイル
├── .claude/
│   ├── settings.local.json
│   └── commands/       ← Skills（/review など）
├── curriculum/         ← カリキュラム（8章）
├── exercises/          ← 演習問題（6セット）
├── templates/          ← レポートのテンプレート
└── output/             ← ここにレポートが生成される
```

### 2) Claude Code の起動テスト

```bash
cd claude-code-textbook-vol2
claude
```

Claude Code が起動し、対話モードになれば成功です。

### 3) レビュアーの動作確認

起動したら、以下を試してみてください:

```
/review README.md
```

フィードバックが返ってくれば、レビュアー Skill が正常に動作しています。

---

## 4. Vol.1 未受講の方へ

Vol.1 を受けていなくても、以下の基本操作がわかっていれば受講可能です:

- Claude Code の起動と終了（`claude` / `Ctrl+C`）
- ファイル操作の依頼（「〇〇を読んで」「〇〇を作成して」）
- CLAUDE.md の役割（プロジェクトルールの定義）
- Skills の概念（`.claude/commands/` にカスタムコマンドを置く）
- Subagents の概念（Claude Code が複雑なタスクを並列処理する仕組み）

不安な場合は Chapter 1（復習）で基礎を確認できるので、そのまま進めて大丈夫です。

---

## 困ったときは

セットアップで問題が発生した場合は、以下のメールアドレスまでご連絡ください。

**連絡先**: kohei812045x@gmail.com
