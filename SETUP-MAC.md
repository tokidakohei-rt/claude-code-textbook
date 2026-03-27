# Mac セットアップガイド — Claude Code を使い始めるまで

このガイドの手順に沿って進めるだけで、Mac で Claude Code を使い始めるための環境が整います。

---

## 目次

1. [必要なものリスト](#1-必要なものリスト)
2. [ターミナルを開く](#2-ターミナルを開く)
3. [Homebrew のインストール](#3-homebrew-のインストール)
4. [Git のインストールと初期設定](#4-git-のインストールと初期設定)
5. [GitHub アカウントの作成と認証](#5-github-アカウントの作成と認証)
6. [Node.js のインストール](#6-nodejs-のインストール)
7. [VS Code または Cursor のインストール](#7-vs-code-または-cursor-のインストール)
8. [Claude Code のインストール](#8-claude-code-のインストール)
9. [リポジトリのクローン（取り込み）](#9-リポジトリのクローン取り込み)
10. [トラブルシューティング](#10-トラブルシューティング)

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

## 2. ターミナルを開く

Mac に最初から入っている「ターミナル」アプリを使います。

1. `Cmd + Space` を押して **Spotlight** を起動
2. 「ターミナル」と入力
3. 表示された **ターミナル.app** を選択して Enter

> 以降の手順はすべてこのターミナル上で実行します。

---

## 3. Homebrew のインストール

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

## 4. Git のインストールと初期設定

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

## 5. GitHub アカウントの作成と認証

### アカウント作成（すでにある方はスキップ）

1. [https://github.com](https://github.com) にアクセス
2. 右上の **「Sign up」** をクリック
3. メールアドレス、パスワード、ユーザー名を入力
4. メール認証を完了
5. 無料プラン（Free）のままでOK

### GitHub CLI のインストール（推奨）

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

## 6. Node.js のインストール

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

## 7. VS Code または Cursor のインストール

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

## 8. Claude Code のインストール

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

## 9. リポジトリのクローン（取り込み）

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

## 10. トラブルシューティング

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
