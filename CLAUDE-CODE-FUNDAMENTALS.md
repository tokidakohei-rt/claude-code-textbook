# Claude Code 基礎知識ガイド

Claude Code は Anthropic 公式のターミナルベース AI エージェントです。コードの生成・修正・テスト・レビューから、Git 操作や外部ツール連携まで、ソフトウェア開発のあらゆるタスクを自然言語で指示できます。

---

## 目次

1. [基本操作](#1-基本操作)
2. [ツールと権限モデル](#2-ツールと権限モデル)
3. [CLAUDE.md（ルール設定）](#3-claudemdルール設定)
4. [Skills（カスタムコマンド）](#4-skillsカスタムコマンド)
5. [Subagents（並列エージェント）](#5-subagents並列エージェント)
6. [MCP（外部ツール連携）](#6-mcp外部ツール連携)
7. [Hooks（イベント駆動の自動処理）](#7-hooksイベント駆動の自動処理)
8. [Git 連携](#8-git-連携)
9. [IDE 連携](#9-ide-連携)
10. [コンテキスト管理](#10-コンテキスト管理)
11. [設定ファイル構成](#11-設定ファイル構成)
12. [キーボードショートカット](#12-キーボードショートカット)
13. [セキュリティ](#13-セキュリティ)

---

## 1. 基本操作

### インストール

```bash
npm install -g @anthropic-ai/claude-code
```

### 起動と終了

```bash
claude              # カレントディレクトリで起動
claude <directory>  # 指定ディレクトリで起動
```

- `/exit` または `Ctrl+C` で終了

### 認証

初回起動時に認証を求められます。以下のいずれかが必要です:

- **Anthropic API キー**（従量課金）
- **Claude Pro / Max プラン**（月額定額）

### 主要なスラッシュコマンド

| コマンド | 説明 |
|---------|------|
| `/help` | ヘルプを表示 |
| `/exit` | セッション終了 |
| `/status` | セッション状態の確認 |
| `/cost` | トークン使用量と費用の確認 |
| `/compact` | コンテキストを圧縮（古い会話を要約） |
| `/memory` | CLAUDE.md や自動メモリの表示・編集 |
| `/sessions` | 保存済みセッション一覧 |
| `/resume` | 過去のセッションを再開 |
| `/mcp` | MCP サーバーの管理 |
| `/hooks` | Hooks の管理 |

---

## 2. ツールと権限モデル

Claude Code は各種「ツール」を通じてファイル操作やコマンド実行を行います。

### 主要ツール

| ツール | 用途 | リスク |
|-------|------|--------|
| `Read` | ファイルの読み取り | 低（情報の閲覧のみ） |
| `Edit` | ファイルの部分編集 | 中（既存ファイルの変更） |
| `Write` | ファイルの新規作成・全体上書き | 中（上書きの可能性） |
| `Bash` | ターミナルコマンドの実行 | 高（任意のコマンド実行） |
| `Glob` | ファイルパターン検索 | 低 |
| `Grep` | ファイル内容の検索 | 低 |
| `WebFetch` | Web ページの取得 | 中 |
| `WebSearch` | Web 検索 | 低 |
| `Agent` | Subagent の起動 | 中 |

### ツール承認モデル

Claude Code がツールを使おうとすると、リスクレベルに応じて承認を求めます。

- **自動許可**: `Read`、`Glob`、`Grep` など読み取り系
- **確認が必要**: `Edit`、`Write`、`Bash` など変更・実行系

承認プロンプトが表示されたら:
- `y` → 今回だけ許可
- `Y`（大文字）→ このセッション中は同種の操作を自動許可
- `n` → 拒否

### 権限モードの設定

`settings.json` で権限を細かく制御できます:

```json
{
  "permissions": {
    "allow": [
      "Bash(npm test)",
      "Bash(npm run build)",
      "Read(src/**)",
      "Edit(src/**)"
    ],
    "deny": [
      "Bash(rm -rf *)",
      "Bash(sudo *)",
      "Write(/etc/**)"
    ]
  }
}
```

- **allow**: 確認なしで自動許可するパターン
- **deny**: 常にブロックするパターン
- パターンには `*` や `**` のワイルドカードが使用可能

---

## 3. CLAUDE.md（ルール設定）

### CLAUDE.md とは

Claude Code がプロジェクトの指示・コンテキストを読み込むマークダウンファイルです。「このプロジェクトではこう振る舞ってほしい」というルールを書いておくと、毎回説明しなくても Claude Code が従います。

### 階層構造と優先順位

CLAUDE.md は複数の場所に配置でき、階層的に読み込まれます:

```
~/.claude/CLAUDE.md              ← ユーザーレベル（全プロジェクト共通）
<repo>/CLAUDE.md                 ← プロジェクトレベル（リポジトリ全体）
<repo>/.claude/CLAUDE.md         ← プロジェクトローカル（gitignore 推奨）
<repo>/src/CLAUDE.md             ← ディレクトリレベル（特定フォルダ）
```

**優先順位**: ディレクトリレベル > プロジェクトレベル > ユーザーレベル

### 効果的な CLAUDE.md の書き方

```markdown
# プロジェクト名

## 概要
このプロジェクトは ○○ です。

## 技術スタック
- TypeScript + React
- Node.js v18+
- PostgreSQL

## 重要なルール
- テストは必ず書くこと
- コミットメッセージは日本語で書く
- `src/` 配下のみ編集可能

## 禁止事項
- `build/` ディレクトリは手動編集しない（自動生成）
- `.env` ファイルをコミットしない

## 出力形式
- コードにはコメントを最小限にする
- 関数には JSDoc を付与する
```

**ポイント**:
- 役割・ルール・禁止事項・出力形式の 4 要素を意識する
- 具体的に書く（「きれいなコード」ではなく「ESLint ルールに従う」）
- 長すぎるとコンテキストを圧迫するので簡潔に

---

## 4. Skills（カスタムコマンド）

### Skills とは

繰り返し使う処理をカスタムスラッシュコマンドとして定義できる機能です。

### ファイル配置

```
~/.claude/commands/              ← ユーザーレベル（全プロジェクト共通）
<repo>/.claude/commands/         ← プロジェクトレベル
```

### Skill の作り方

`.claude/commands/` に Markdown ファイルを配置します。ファイル名がコマンド名になります。

**例: `.claude/commands/summarize.md`**

```markdown
以下のファイルを読み込み、要約してください。

$ARGUMENTS

要約のルール:
- 3〜5行で簡潔にまとめる
- 専門用語は平易な言葉に置き換える
- 重要なポイントを箇条書きにする
```

**使い方**:

```
/summarize src/auth/login.ts
```

- `$ARGUMENTS` はコマンド実行時に渡された引数に置き換わる
- サブディレクトリも使える（例: `.claude/commands/report/weekly.md` → `/report:weekly`）

---

## 5. Subagents（並列エージェント）

### Subagents とは

Claude Code が独立した AI エージェントを別プロセスとして起動し、複雑なタスクを分割・並列実行する仕組みです。

### 仕組み

```
メインエージェント: 「このバグを修正して」
  ├→ Subagent A (調査): ログを調べて原因を特定
  ├→ Subagent B (テスト): テストケースを作成
  └→ 結果を集約して修正を完了
```

各 Subagent は **独立したコンテキスト** を持つため、メインのコンテキストを消費しません。

### 主要な Subagent タイプ

| タイプ | 用途 |
|-------|------|
| `general-purpose` | 汎用的な調査・実行 |
| `Explore` | コードベースの探索・検索 |
| `Plan` | 実装計画の設計 |
| `Bash` | シェルコマンドの実行 |

### Subagent が自動起動される場面

- 複数ファイルの同時調査が必要なとき
- 大規模なリファクタリング
- 並列で独立したタスクを処理するとき

Claude Code が状況に応じて自動的に判断して Subagent を起動します。ユーザーが明示的に指示することもできます。

---

## 6. MCP（外部ツール連携）

### MCP とは

**Model Context Protocol** — Claude Code が外部のツール・API・データベースに接続するための標準化プロトコルです。

### 設定ファイル

```
~/.claude/.mcp.json              ← ユーザーレベル
<repo>/.claude/.mcp.json         ← プロジェクトレベル
```

### 設定例

```json
{
  "mcpServers": {
    "github": {
      "type": "stdio",
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "${GITHUB_TOKEN}"
      }
    }
  }
}
```

### 主な MCP サーバー

| サーバー | 用途 |
|---------|------|
| GitHub | リポジトリ操作、Issue・PR 管理 |
| Slack | メッセージ送受信 |
| PostgreSQL | データベースクエリ |
| Sentry | エラーモニタリング |
| ファイルシステム | 外部ディレクトリへのアクセス |

MCP サーバーの一覧: [github.com/modelcontextprotocol/servers](https://github.com/modelcontextprotocol/servers)

---

## 7. Hooks（イベント駆動の自動処理）

### Hooks とは

Claude Code の特定イベント時にシェルスクリプトや外部 API を自動実行する仕組みです。

### 対応イベント

| イベント | タイミング |
|---------|-----------|
| `PreToolUse` | ツール実行前 |
| `PostToolUse` | ツール実行後 |
| `SessionStart` | セッション開始時 |
| `Stop` | セッション終了時 |

### 設定例（settings.json 内）

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash(rm.*)",
        "hooks": [
          {
            "type": "command",
            "command": "echo '削除コマンドが実行されようとしています'"
          }
        ]
      }
    ]
  }
}
```

### 用途

- 危険なコマンドの検知・ブロック
- ファイル変更時の自動フォーマット
- セッション開始時の環境チェック

---

## 8. Git 連携

Claude Code は Git リポジトリを自動検出し、以下の操作が可能です:

### 基本操作

```
「このバグを修正してコミットして」
「feature/auth ブランチを作って」
「PR を作成して」
```

### 安全性のデフォルト動作

Claude Code は Git 操作において以下の安全策をデフォルトで取ります:

- **amend より新規コミットを優先** — 既存コミットの書き換えリスクを回避
- **force push の前に確認** — `--force` は明示的な指示がない限り使わない
- **機密ファイルのコミット防止** — `.env` 等をコミットしようとすると警告
- **破壊的操作の前に確認** — `git reset --hard`、ブランチ削除など

### GitHub Actions との連携

CI/CD パイプラインから Claude Code を呼び出し、自動テスト・自動修正・自動レビューが可能です。

---

## 9. IDE 連携

### VS Code

1. 拡張機能マーケットプレイスで「Claude Code」を検索・インストール
2. サイドバーに Claude Code パネルが表示される
3. `Cmd+Shift+A` で Prompt Box を開いて対話

### Cursor

Claude Code のターミナル統合をそのまま利用可能。Cursor 内蔵ターミナルで `claude` を起動。

### JetBrains IDE

IntelliJ、PyCharm、WebStorm 等で利用可能。Preferences → Plugins から「Claude Code」をインストール。

---

## 10. コンテキスト管理

### コンテキストウィンドウとは

Claude が一度の会話で処理できるトークン（テキスト）の上限です。会話が長くなるとこの上限に近づきます。

### コンテキストの節約テクニック

| テクニック | 方法 |
|-----------|------|
| `/compact` | 古い会話を要約して圧縮 |
| Subagent の活用 | 独立したコンテキストで処理 |
| セッション分割 | 大きなタスクは複数セッションに |
| CLAUDE.md を簡潔に | 長すぎるルールはコンテキストを圧迫 |

### セッションの再開

```bash
/sessions          # 過去のセッション一覧
/resume <id>       # 特定のセッションを再開
```

---

## 11. 設定ファイル構成

### ~/.claude/ ディレクトリ

```
~/.claude/
├── CLAUDE.md              # グローバルルール
├── settings.json          # グローバル設定（権限、Hooks 等）
├── keybindings.json       # キーボードショートカット
├── .mcp.json              # MCP サーバー設定
├── commands/              # グローバル Skills
│   └── summarize.md
└── projects/              # プロジェクト別の自動メモリ
```

### プロジェクト内の .claude/

```
<repo>/.claude/
├── CLAUDE.md              # プロジェクトローカルルール
├── settings.json          # プロジェクト設定
├── .mcp.json              # プロジェクト用 MCP サーバー
└── commands/              # プロジェクト用 Skills
    └── deploy.md
```

### settings.json の主要項目

```json
{
  "permissions": {
    "allow": ["Bash(npm *)"],
    "deny": ["Bash(sudo *)"]
  },
  "hooks": { },
  "env": {
    "NODE_ENV": "development"
  }
}
```

---

## 12. キーボードショートカット

### ターミナル（対話モード）

| キー | 機能 |
|-----|------|
| `Enter` | メッセージ送信 |
| `Ctrl+C` | 現在の操作をキャンセル |
| `Ctrl+D` | セッション終了 |
| `Ctrl+L` | 画面クリア |
| `↑` / `↓` | 入力履歴の操作 |
| `Tab` | オートコンプリート |
| `Esc` | 入力の複数行モード切り替え |

### VS Code

| キー | 機能 |
|-----|------|
| `Cmd+Shift+A` | Prompt Box を開く |
| `Cmd+Enter` | メッセージ送信 |

---

## 13. セキュリティ

Claude Code は強力なツールであるため、セキュリティを意識した運用が重要です。

### 13.1 API キーの安全な管理

#### やってはいけないこと

```bash
# ❌ コードにハードコード
API_KEY="sk-ant-1234567890abcdef"

# ❌ Git にコミット
git add .env
git commit -m "add config"
```

#### 安全な方法

```bash
# ✅ 環境変数として設定（~/.zshrc に追加）
export ANTHROPIC_API_KEY="sk-ant-..."

# ✅ .gitignore で除外
echo ".env" >> .gitignore
echo ".env.local" >> .gitignore
echo "*.key" >> .gitignore
```

### 13.2 権限設定のベストプラクティス

**最小権限の原則** — 必要最低限の権限だけを付与します。

```json
{
  "permissions": {
    "allow": [
      "Bash(npm test)",
      "Bash(npm run build)",
      "Read(src/**)",
      "Edit(src/**)"
    ],
    "deny": [
      "Bash(rm -rf *)",
      "Bash(sudo *)",
      "Bash(curl * | bash)",
      "Bash(chmod 777 *)",
      "Write(/etc/**)",
      "Read(*.pem)",
      "Read(*/.ssh/*)"
    ]
  }
}
```

**段階的に権限を広げる**:

1. まず読み取りのみ許可
2. 信頼できたら編集を許可
3. 必要に応じてコマンド実行を許可

### 13.3 CLAUDE.md インジェクション対策

悪意のある CLAUDE.md がリポジトリに含まれていると、Claude Code が意図しない動作をする可能性があります。

#### 対策

- **クローン直後に CLAUDE.md を確認する** — 特に信頼できないリポジトリ
- **以下のような内容が含まれていたら警戒する**:
  - `sudo` コマンドの実行指示
  - API キーやトークンの入力・送信指示
  - 外部スクリプトのダウンロード・実行（`curl ... | bash`）
  - 特定のファイルの削除指示
- **信頼できないリポジトリの CLAUDE.md を除外する**:

```json
{
  "excludedCLAUDEFiles": [
    "untrusted-repo/**/CLAUDE.md"
  ]
}
```

### 13.4 ファイルアクセス制御

機密ファイルへのアクセスを制限します:

```json
{
  "permissions": {
    "deny": [
      "Read(/etc/passwd)",
      "Read(/etc/shadow)",
      "Read(**/.ssh/*)",
      "Read(**/.aws/credentials)",
      "Read(**/*.key)",
      "Read(**/*.pem)",
      "Read(**/.env)",
      "Read(**/.env.local)"
    ]
  }
}
```

### 13.5 ネットワークアクセスの制御

外部への通信を必要なものだけに制限します:

```json
{
  "permissions": {
    "allow": [
      "WebFetch(https://api.github.com/*)",
      "WebFetch(https://registry.npmjs.org/*)"
    ],
    "deny": [
      "WebFetch(http://*)"
    ]
  }
}
```

**ポイント**: `http://`（暗号化なし）は原則ブロックし、`https://` のみ許可する。

### 13.6 信頼できないリポジトリでの注意点

外部のリポジトリを扱う場合は、以下をチェックしてから Claude Code を起動してください:

```bash
# 1. CLAUDE.md に怪しい内容がないか
cat CLAUDE.md
cat .claude/CLAUDE.md

# 2. .claude/ ディレクトリの内容
ls -la .claude/

# 3. MCP サーバーが勝手に設定されていないか
cat .claude/.mcp.json

# 4. Hooks が仕込まれていないか
cat .claude/settings.json

# 5. Skills に怪しいコマンドがないか
ls -la .claude/commands/
cat .claude/commands/*.md
```

### 13.7 コマンド実行のリスク管理

Claude Code の `Bash` ツールは任意のコマンドを実行できるため、最もリスクが高いツールです。

#### 危険なコマンドパターン

| パターン | リスク |
|---------|--------|
| `rm -rf /` | システム全体の削除 |
| `sudo ...` | 管理者権限での任意実行 |
| `curl ... \| bash` | 外部スクリプトの無検証実行 |
| `chmod 777 ...` | 権限の過剰な開放 |
| `git push --force` | リモートの履歴を破壊 |
| `dd if=/dev/zero ...` | ディスクの破壊 |

#### 対策

1. **deny リストで危険なパターンをブロック**（前述の設定例を参照）
2. **Hooks で実行前にチェック**:

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "hooks": [
          {
            "type": "command",
            "command": "/path/to/validate-command.sh"
          }
        ]
      }
    ]
  }
}
```

3. **承認プロンプトを活用** — 不明なコマンドは `n` で拒否してから内容を確認

### 13.8 セキュリティチェックリスト

Claude Code を安全に使うためのチェックリストです:

- [ ] API キーは環境変数で管理し、コードにハードコードしない
- [ ] `.env`、`.key`、`.pem` は `.gitignore` に追加
- [ ] `settings.json` の `deny` リストに危険なコマンドを登録
- [ ] 信頼できないリポジトリでは CLAUDE.md を事前確認
- [ ] `sudo` を含むコマンドは原則ブロック
- [ ] HTTP（非暗号化）通信はブロック
- [ ] 機密ファイル（SSH 鍵、AWS 認証情報等）への読み取りをブロック
- [ ] 定期的に `/inspect` で現在の権限設定を確認

---

## 参考リンク

- [Claude Code 公式ドキュメント](https://docs.anthropic.com/en/docs/claude-code/overview)
- [Claude Code GitHub](https://github.com/anthropics/claude-code)
- [MCP サーバー一覧](https://github.com/modelcontextprotocol/servers)
