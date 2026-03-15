# セットアップガイド

このガイドの手順に沿って進めるだけで、非エンジニアでも最速で最先端の AI エージェントを使いこなすための知識と環境セットアップが完了します。

本教材では、Anthropic 公式の CLI ツール「Claude Code」を使い、論文を読み解いてスライドにまとめるまでを約2時間で体験します。さらに、Claude Code を使いこなす上で欠かせない **Skills**（カスタムコマンド）や **Subagents**（並列エージェント）といった概念も学び、実際に動かしてみることができます。

---

## 目次

1. [必要なものリスト](#1-必要なものリスト)
2. [GitHub アカウントの作成](#2-github-アカウントの作成)
3. [Git のインストール](#3-git-のインストール)
4. [GitHub CLI のインストール](#4-github-cli-のインストール)
5. [VS Code または Cursor のインストール](#5-vs-code-または-cursor-のインストール)
6. [Claude Code のインストール](#6-claude-code-のインストール)
7. [このリポジトリの取り込み（クローン）](#7-このリポジトリの取り込みクローン)
8. [動作確認](#8-動作確認)
9. [トラブルシューティング](#9-トラブルシューティング)

---

## 1. 必要なものリスト

| 項目 | 必須/推奨 | 備考 |
|------|----------|------|
| GitHub アカウント | 必須 | 無料プランでOK |
| Git | 必須 | バージョン管理ツール |
| GitHub CLI (`gh`) | 推奨 | GitHub の認証が簡単になる |
| VS Code または Cursor | 必須 | コードエディタ（どちらか1つ） |
| Node.js (v18以上) | 必須 | Claude Code と Marp CLI に必要 |
| Claude Code | 必須 | Anthropic のCLIツール |
| Anthropic API キーまたは Claude Pro/Max プラン | 必須 | Claude Code の利用に必要 |
| 要約したい論文（PDF） | 任意 | 自分の好きな論文を用意してください |

---

## 2. GitHub アカウントの作成

すでにアカウントがある方はスキップしてください。

1. [https://github.com](https://github.com) にアクセス
2. 右上の **「Sign up」** をクリック
3. メールアドレス、パスワード、ユーザー名を入力
4. メール認証を完了
5. 無料プラン（Free）のままでOK

---

## 3. Git のインストール

### Mac の場合

まずターミナルを開きます：

1. `Cmd + Space` を押して **Spotlight** を起動
2. 「ターミナル」と入力
3. 表示された **ターミナル.app** を選択して Enter

ターミナルが開いたら、まず **Homebrew**（Mac 用のパッケージ管理ツール）をインストールします：

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

> すでに Homebrew が入っている場合は「Already installed」と表示されるので、そのまま次へ進んでください。

Homebrew が入ったら、Git をインストールします：

```bash
brew install git
```

### Windows の場合

1. [https://git-scm.com/downloads/win](https://git-scm.com/downloads/win) からインストーラをダウンロード
2. インストーラを実行（基本的にデフォルト設定のままでOK）
3. 「Git Bash」がインストールされたことを確認

### インストール確認

```bash
git --version
# 例: git version 2.43.0
```

### Git の初期設定

```bash
git config --global user.name "あなたのユーザー名"
git config --global user.email "あなたのメールアドレス"
```

### 🎨 おすすめ: Starship でターミナルを見やすくする（任意）

**Starship** を入れると、ターミナルに今いるフォルダや Git ブランチがカラフルに表示されるようになります。「今どこにいるの？」「どのブランチ？」が一目でわかるので、初心者の方にもおすすめです。

```bash
brew install starship
```

インストール後、以下を実行してシェルに設定を追加します：

```bash
echo 'eval "$(starship init zsh)"' >> ~/.zshrc
source ~/.zshrc
```

設定後、ターミナルのプロンプトが変わり、ディレクトリ名や Git ブランチ名が色付きで表示されるようになります。

---

## 4. GitHub CLI のインストール

GitHub CLI (`gh`) は Git とは別のツールで、GitHub への認証やリポジトリ操作を簡単にしてくれます。Git だけでもクローンは可能ですが、プライベートリポジトリへのアクセスや認証で便利なのでインストールを推奨します。

### Mac の場合

```bash
brew install gh
```

### Windows の場合

1. [https://cli.github.com](https://cli.github.com) からインストーラをダウンロード
2. インストーラを実行

### 認証（GitHub へのログイン）

```bash
gh auth login
```

対話形式で以下を選択：
- `GitHub.com` を選択
- プロトコル: `HTTPS` を選択
- ブラウザで認証: `Y` を選択

ブラウザが開くので、GitHub にログインして認証を許可してください。これで `git clone` や `git push` がスムーズに使えるようになります。

### インストール確認

```bash
gh --version
# 例: gh version 2.45.0
```

---

## 5. VS Code または Cursor のインストール

どちらか一方を選んでください。初めての方には **VS Code** をおすすめします。

### VS Code

1. [https://code.visualstudio.com](https://code.visualstudio.com) にアクセス
2. 自分のOS用のインストーラをダウンロード
3. インストール
4. 起動して日本語化したい場合は、拡張機能で「Japanese Language Pack」を検索してインストール

### Cursor

1. [https://cursor.com](https://cursor.com) にアクセス
2. ダウンロード＆インストール
3. VS Code とほぼ同じ操作感で使えます

---

## 6. Claude Code のインストール

Claude Code はターミナル上で動く Anthropic 公式の AI アシスタントです。

### 前提条件

- **Node.js v18 以上** が必要です

Node.js が入っていない場合：

```bash
# Mac（Homebrew）
brew install node

# または公式サイトからダウンロード
# https://nodejs.org
```

バージョン確認：

```bash
node --version
# v18.x.x 以上であればOK
```

### Claude Code のインストール

```bash
npm install -g @anthropic-ai/claude-code
```

### 認証

Claude Code を使うには、以下のいずれかが必要です：

- **Anthropic API キー**（従量課金）
- **Claude Pro / Max プラン**（月額定額でClaude Codeを利用可能）

```bash
# 初回起動時に認証を求められます
claude
```

画面の指示に従って認証を完了してください。

> **補足**: APIキーを使う場合の料金目安は、2時間程度の利用で数百円〜千円程度です。Claude Max プランの場合は追加料金はかかりません。

---

## 7. このリポジトリの取り込み（クローン）

### 方法A: ターミナルから（おすすめ）

```bash
# 好きなフォルダに移動（例: ホームディレクトリ）
cd ~

# クローン
git clone https://github.com/tokidakohei-rt/claude-code-textbook.git

# フォルダに移動
cd claude-code-textbook
```

### 方法B: VS Code / Cursor から

1. VS Code（またはCursor）を開く
2. `Ctrl+Shift+P`（Mac: `Cmd+Shift+P`）でコマンドパレットを開く
3. 「Git: Clone」と入力して選択
4. リポジトリURL を入力:
   ```
   https://github.com/tokidakohei-rt/claude-code-textbook.git
   ```
5. 保存先フォルダを選択
6. 「Open」（開く）を選択

### 方法C: GitHub から ZIP でダウンロード

Git に不慣れな場合はこちらでもOKです。

1. [リポジトリページ](https://github.com/tokidakohei-rt/claude-code-textbook) にアクセス
2. 緑色の **「Code」** ボタンをクリック
3. **「Download ZIP」** を選択
4. ダウンロードしたZIPを展開
5. VS Code / Cursor で展開したフォルダを開く

---

## 8. 動作確認

クローンしたフォルダで以下を順に実行してください。

### 1) フォルダ構成の確認

VS Code / Cursor でフォルダを開き、以下の構成になっていることを確認：

```
claude-code-textbook/
├── CLAUDE.md          ← Claude Code 用の指示ファイル
├── SETUP.md           ← このファイル
├── curriculum/        ← カリキュラム（章ごとの教材）
├── exercises/         ← 演習問題
├── templates/         ← スライドのテンプレート
├── paper/             ← ここに論文PDFを配置（空の状態）
└── output/            ← ここにスライドが生成される（空の状態）
```

### 2) Claude Code の起動テスト

ターミナル（VS Code内のターミナルでもOK）で：

```bash
# リポジトリのフォルダ内で実行
cd claude-code-textbook
claude
```

Claude Code が起動し、対話モードになれば成功です。

### 3) 試しに話しかけてみる

Claude Code が起動したら、以下を入力してみてください：

```
このリポジトリの構成を教えて
```

ファイル一覧を返してくれれば、正常に動作しています。
回答の末尾に「カリキュラムを始めますか？」と表示されるので、そのまま「はい」と答えればカリキュラムがスタートします。

`Ctrl+C` または `/exit` で終了できます。

---

## 9. トラブルシューティング

### `git clone` でエラーが出る

```
fatal: could not read Username for 'https://github.com': terminal prompts disabled
```

→ GitHub への認証ができていません。[4. GitHub CLI のインストール](#4-github-cli-のインストール) の手順で `gh auth login` を実行してください。

### `npm install -g` で Permission エラーが出る

```bash
# Mac/Linux の場合
sudo npm install -g @anthropic-ai/claude-code

# または Node.js のバージョン管理ツール（nvm）を使うのがおすすめ
# https://github.com/nvm-sh/nvm
```

### `claude` コマンドが見つからない

```bash
# パスが通っているか確認
which claude

# 見つからない場合、npm のグローバルパスを確認
npm config get prefix
# 表示されたパス/bin がPATHに含まれているか確認
```

### Node.js のバージョンが古い

```bash
node --version
# v18 未満の場合はアップデートが必要

# nvm を使っている場合
nvm install --lts
nvm use --lts
```

---

## 困ったときは

セットアップで問題が発生した場合は、以下のメールアドレスまでご連絡ください。

**連絡先**: kohei812045x@gmail.com
