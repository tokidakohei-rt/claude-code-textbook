# Mac セットアップガイド — Claude Code を使い始めるまで

このガイドの手順に沿って進めるだけで、Mac で Claude Code を使い始めるための環境が整います。

---

## 目次

1. [必要なものリスト](#1-必要なものリスト)
2. [GitHub アカウントの作成](#2-github-アカウントの作成)
3. [ターミナルを開く](#3-ターミナルを開く)
4. [Homebrew のインストール](#4-homebrew-のインストール)
5. [Git のインストールと初期設定](#5-git-のインストールと初期設定)
6. [GitHub CLI のインストールと認証](#6-github-cli-のインストールと認証)
7. [Node.js のインストール](#7-nodejs-のインストール)
8. [VS Code または Cursor のインストール](#8-vs-code-または-cursor-のインストール)
9. [Claude Code のインストール](#9-claude-code-のインストール)
10. [リポジトリのクローン（取り込み）](#10-リポジトリのクローン取り込み)
11. [トラブルシューティング](#11-トラブルシューティング)
12. [覚えておくと便利なターミナルコマンド集](#12-覚えておくと便利なターミナルコマンド集)

---

## 1. 必要なものリスト

| 項目 | 必須/推奨 | 備考 |
|------|----------|------|
| Homebrew | 必須 | Mac 用パッケージ管理ツール |
| Git | 必須 | バージョン管理ツール |
| GitHub アカウント | 必須 | 無料プランでOK |
| GitHub CLI (`gh`) | 推奨 | GitHub への認証が簡単になる |
| Node.js (v18以上) | 必須 | Claude Code の動作に必要 |
| VS Code または Cursor | 必須 | コードエディタ（どちらか1つ） |
| Claude Code | 必須 | Anthropic 公式の CLI ツール |
| Anthropic API キーまたは Claude Pro/Max プラン | 必須 | Claude Code の利用に必要 |

---

## 2. GitHub アカウントの作成

> すでにアカウントをお持ちの方はスキップしてください。

1. [https://github.com](https://github.com) にアクセス
2. 右上の **「Sign up」** をクリック
3. メールアドレス、パスワード、ユーザー名を入力
4. メール認証を完了
5. 無料プラン（Free）のままでOK

> この後のステップで、ここで作ったユーザー名とメールアドレスを使います。

---

## 3. ターミナルを開く

Mac に最初から入っている「ターミナル」アプリを使います。

1. `Cmd + Space` を押して **Spotlight** を起動
2. 「ターミナル」と入力
3. 表示された **ターミナル.app** を選択して Enter

> 以降の手順はすべてこのターミナル上で実行します。

---

## 4. Homebrew のインストール

**Homebrew** は Mac 用のパッケージ管理ツールです。Git や Node.js など、開発に必要なツールをコマンド1つでインストールできるようになります。

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

> すでにインストール済みの場合は「Already installed」と表示されます。そのまま次へ進んでください。

インストール後、ターミナルに表示される指示に従って PATH を通してください（Apple Silicon Mac の場合、以下のような案内が出ることがあります）:

```bash
echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> ~/.zshrc
source ~/.zshrc
```

### インストール確認

```bash
brew --version
```

> 例: `Homebrew 4.x.x` のように表示されます

---

## 5. Git のインストールと初期設定

### インストール

```bash
brew install git
```

### インストール確認

```bash
git --version
```

> 例: `git version 2.43.0` のように表示されます

### 初期設定

GitHub で使っているユーザー名とメールアドレスを設定します:

```bash
git config --global user.name "あなたのユーザー名"
git config --global user.email "あなたのメールアドレス"
```

### おすすめ: Starship でターミナルを見やすくする（任意）

**Starship** を入れると、ターミナルに今いるフォルダや Git ブランチがカラフルに表示されます。「今どこにいるの？」「どのブランチ？」が一目でわかるので、初心者の方にもおすすめです。

```bash
brew install starship
```

インストール後、以下を実行してシェルに設定を追加します:

```bash
echo 'eval "$(starship init zsh)"' >> ~/.zshrc
source ~/.zshrc
```

---

## 6. GitHub CLI のインストールと認証

GitHub CLI (`gh`) を入れると、GitHub への認証やリポジトリ操作がスムーズになります。

```bash
brew install gh
```

### GitHub への認証

```bash
gh auth login
```

対話形式で以下を選択:
- `GitHub.com` を選択
- プロトコル: `HTTPS` を選択
- ブラウザで認証: `Y` を選択

ブラウザが開くので、GitHub にログインして認証を許可してください。

### インストール確認

```bash
gh --version
```

> 例: `gh version 2.45.0` のように表示されます

---

## 7. Node.js のインストール

Claude Code の動作には **Node.js v18 以上** が必要です。

```bash
brew install node
```

### インストール確認

```bash
node --version
```

> `v18.x.x` 以上であればOKです

> すでにインストール済みで v18 未満の場合は `brew upgrade node` でアップデートできます。

---

## 8. VS Code または Cursor のインストール

コードエディタは **VS Code** か **Cursor** のどちらかを選んでください。初めての方には VS Code をおすすめします。

### VS Code の場合

1. [https://code.visualstudio.com](https://code.visualstudio.com) にアクセス
2. **「Download for Mac」** をクリック
3. ダウンロードした `.zip` を展開し、アプリケーションフォルダにドラッグ
4. 起動後、日本語化したい場合は拡張機能で「Japanese Language Pack」を検索してインストール

### Cursor の場合

1. [https://cursor.com](https://cursor.com) にアクセス
2. ダウンロード＆インストール
3. VS Code とほぼ同じ操作感で使えます

---

## 9. Claude Code のインストール

### インストール

```bash
npm install -g @anthropic-ai/claude-code
```

### 認証

Claude Code を使うには、以下のいずれかが必要です:

- **Anthropic API キー**（従量課金）
- **Claude Pro / Max プラン**（月額定額で Claude Code を利用可能）

初回起動時に認証を求められます:

```bash
claude
```

画面の指示に従って認証を完了してください。

> **補足**: API キーを使う場合の料金目安は、2時間程度の利用で数百円〜千円程度です。Claude Max プランの場合は追加料金はかかりません。

### インストール確認

```bash
claude --version
```

---

## 10. リポジトリのクローン（取り込み）

GitHub 上のリポジトリを自分の Mac に取り込む方法です。

### 方法A: VS Code / Cursor からクローンする（おすすめ）

1. VS Code（または Cursor）を開く
2. `Cmd + Shift + P` でコマンドパレットを開く
3. 「**Git: Clone**」と入力して選択
4. クローンしたいリポジトリの URL を入力（例: `https://github.com/ユーザー名/リポジトリ名.git`）
5. 保存先フォルダを選択（ホームディレクトリやデスクトップなど）
6. クローン完了後、「**Open**」（開く）を選択

これでエディタ上にリポジトリのファイルが表示されます。

### 方法B: ターミナルからクローンする

保存したいフォルダに移動（例: ホームディレクトリ）してからクローンします:

```bash
cd ~
git clone https://github.com/ユーザー名/リポジトリ名.git
cd リポジトリ名
```

その後、VS Code / Cursor でフォルダを開きます:

VS Code の場合:

```bash
code .
```

Cursor の場合:

```bash
cursor .
```

> `code` コマンドが使えない場合: VS Code を開き、`Cmd + Shift + P` → 「Shell Command: Install 'code' command in PATH」を実行してください。Cursor も同様に設定できます。

### 方法C: GitHub から ZIP でダウンロード

Git に不慣れな場合はこちらでもOKです。

1. GitHub のリポジトリページにアクセス
2. 緑色の **「Code」** ボタンをクリック
3. **「Download ZIP」** を選択
4. ダウンロードした ZIP を展開
5. VS Code / Cursor で展開したフォルダを開く

---

## 11. トラブルシューティング

### `brew` コマンドが見つからない

Apple Silicon Mac（M1/M2/M3/M4）の場合、Homebrew のパスを通す必要があります:

```bash
echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> ~/.zshrc
source ~/.zshrc
```

### `git clone` でエラーが出る

```
fatal: could not read Username for 'https://github.com': terminal prompts disabled
```

→ GitHub への認証ができていません。`gh auth login` を実行してください。

### `npm install -g` で Permission エラーが出る

```bash
sudo npm install -g @anthropic-ai/claude-code
```

または、Node.js のバージョン管理ツール **nvm** を使うのがおすすめです:
- [https://github.com/nvm-sh/nvm](https://github.com/nvm-sh/nvm)

### `claude` コマンドが見つからない

パスが通っているか確認:

```bash
which claude
```

見つからない場合、npm のグローバルパスを確認:

```bash
npm config get prefix
```

表示されたパス/bin が PATH に含まれているか確認してください。

### Node.js のバージョンが古い

```bash
node --version
```

v18 未満の場合、Homebrew でアップデート:

```bash
brew upgrade node
```

nvm を使っている場合:

```bash
nvm install --lts
nvm use --lts
```

### `code` / `cursor` コマンドが使えない

エディタを開いた状態で `Cmd + Shift + P` → 以下を検索して実行:
- VS Code: 「**Shell Command: Install 'code' command in PATH**」
- Cursor: 「**Shell Command: Install 'cursor' command in PATH**」

---

## 12. 覚えておくと便利なターミナルコマンド集

普段の作業でよく使うコマンドをまとめました。最初から全部覚える必要はありません。必要になったときにここに戻ってきてください。

### ファイル・フォルダ移動系（cd / ls など）

| コマンド | 説明 |
|---------|------|
| `pwd` | 今いるフォルダのパスを表示 |
| `ls` | 今いるフォルダのファイル一覧を表示 |
| `ls -la` | 隠しファイルも含めて詳細表示 |
| `cd フォルダ名` | 指定フォルダへ移動 |
| `cd ~` | ホームディレクトリへ移動 |
| `cd ..` | 1つ上のフォルダへ移動 |
| `cd -` | 直前にいたフォルダへ戻る |
| `open .` | 今いるフォルダを Finder で開く |

### ファイル・フォルダ操作系（mkdir / cp / mv / rm）

| コマンド | 説明 |
|---------|------|
| `mkdir フォルダ名` | フォルダを作成 |
| `mkdir -p a/b/c` | 階層フォルダを一気に作成 |
| `touch ファイル名` | 空ファイルを作成 |
| `cp 元 先` | ファイルをコピー |
| `cp -r 元 先` | フォルダを丸ごとコピー |
| `mv 元 先` | ファイル/フォルダを移動・リネーム |
| `rm ファイル名` | ファイルを削除 |
| `rm -rf フォルダ名` | フォルダを丸ごと削除（取り扱い注意） |
| `cat ファイル名` | ファイルの中身を表示 |

### Git 系

| コマンド | 説明 |
|---------|------|
| `git status` | 変更状況を確認 |
| `git add .` | 変更をすべてステージング |
| `git add ファイル名` | 特定ファイルだけステージング |
| `git commit -m "メッセージ"` | コミットを作成 |
| `git log --oneline` | コミット履歴を1行表示 |
| `git diff` | 変更内容の差分を表示 |
| `git branch` | ブランチ一覧を表示 |
| `git switch ブランチ名` | ブランチを切り替え |
| `git switch -c 新ブランチ名` | 新しいブランチを作成して切り替え |
| `git pull` | リモートの最新を取り込み |
| `git push` | ローカルのコミットをリモートへ送信 |
| `git clone URL` | リポジトリを取り込み |

### GitHub CLI（gh）系

| コマンド | 説明 |
|---------|------|
| `gh auth login` | GitHub に認証 |
| `gh auth status` | 認証状況を確認 |
| `gh repo clone ユーザー名/リポ名` | リポジトリをクローン |
| `gh repo create` | 新しいリポジトリを作成 |
| `gh repo view --web` | 今いるリポジトリを GitHub で開く |
| `gh pr create` | プルリクエストを作成 |
| `gh pr list` | プルリクエスト一覧 |
| `gh pr view 番号 --web` | PR をブラウザで開く |
| `gh issue list` | Issue 一覧 |

### Claude Code 専用

| コマンド | 説明 |
|---------|------|
| `claude` | Claude Code を起動（カレントフォルダで） |
| `claude --version` | バージョン確認 |
| `claude update` | Claude Code を最新版にアップデート |
| `claude doctor` | 環境診断・トラブル時の確認 |
| `claude mcp list` | 設定済みの MCP サーバー一覧 |
| `claude --help` | ヘルプを表示 |
| `npm install -g @anthropic-ai/claude-code` | インストール（または再インストール） |

Claude Code 起動中に使えるスラッシュコマンド（プロンプト欄で `/` を入力）:

| コマンド | 説明 |
|---------|------|
| `/help` | ヘルプ表示 |
| `/clear` | 会話履歴をクリア |
| `/login` | アカウントにログイン |
| `/logout` | ログアウト |
| `/model` | 使用するモデルを切り替え |
| `/cost` | 今セッションの利用料金を確認 |
| `/exit` | Claude Code を終了 |

### Homebrew 系

| コマンド | 説明 |
|---------|------|
| `brew install パッケージ名` | パッケージをインストール |
| `brew uninstall パッケージ名` | パッケージを削除 |
| `brew upgrade` | 全パッケージをアップデート |
| `brew upgrade パッケージ名` | 特定のパッケージだけアップデート |
| `brew list` | インストール済み一覧 |
| `brew search キーワード` | パッケージを検索 |
| `brew doctor` | Homebrew の状態をチェック |

### その他の便利コマンド

| コマンド | 説明 |
|---------|------|
| `clear` または `Ctrl + L` | 画面をクリア |
| `Ctrl + C` | 実行中のコマンドを中断 |
| `Ctrl + R` | 過去のコマンドを検索 |
| `↑` / `↓` | コマンド履歴を辿る |
| `which コマンド名` | コマンドの場所を確認 |
| `man コマンド名` | コマンドのマニュアルを表示（`q` で終了） |
| `code .` | 今いるフォルダを VS Code で開く |
| `cursor .` | 今いるフォルダを Cursor で開く |
