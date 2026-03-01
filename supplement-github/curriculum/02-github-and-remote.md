# Chapter 2: GitHub とリモートリポジトリ

## ローカル vs リモート

Chapter 1 で学んだ `git commit` は、**あなたのPC（ローカル）** にセーブしただけです。PCが壊れたらデータは消えます。

**GitHub** にアップロード（push）することで:
- クラウドにバックアップされる
- 他の人に共有できる
- 別のPCからも作業を引き継げる

## リモートリポジトリの仕組み

```
ローカル（あなたのPC）        リモート（GitHub）
┌──────────────┐              ┌──────────────┐
│  作業ディレクトリ │              │              │
│      ↓ add    │              │  リモート     │
│  ステージング   │   push →    │  リポジトリ   │
│      ↓ commit │              │              │
│  ローカルリポジトリ │  ← pull    │              │
└──────────────┘              └──────────────┘
```

## 覚えるコマンド

### `git clone` — リモートからコピーする

```bash
git clone https://github.com/ユーザー名/リポジトリ名.git
```

GitHub にあるリポジトリを丸ごとローカルにコピーします。このテキストブックも `git clone` で取得したはずです。

### `git push` — ローカルの変更をリモートに送る

```bash
git push origin main
```

`commit` した内容を GitHub にアップロードします。

| 部分 | 意味 |
|------|------|
| `origin` | リモートの名前（デフォルト） |
| `main` | ブランチ名（メインの作業ライン） |

### `git pull` — リモートの変更をローカルに取り込む

```bash
git pull origin main
```

GitHub 上の最新の変更をローカルに取り込みます。他の人が変更を push した場合や、別のPCで作業した後に使います。

### `git remote -v` — リモートの設定を確認する

```bash
git remote -v
```

どの GitHub リポジトリと繋がっているか確認できます。

## GitHub CLI（gh）でリポジトリを作る

ブラウザで GitHub を開かなくても、ターミナルからリポジトリを作れます。

### 新しいリポジトリを作る

```bash
gh repo create my-project --public
```

| オプション | 意味 |
|-----------|------|
| `--public` | 誰でも見られるリポジトリ |
| `--private` | 自分だけが見られるリポジトリ |

### 既存のフォルダをリポジトリにする

```bash
cd my-project
git init
gh repo create my-project --source=. --push
```

これだけで:
1. ローカルで Git を初期化
2. GitHub にリポジトリを作成
3. ファイルをアップロード

が一気に完了します。

## ブランチ — 作業を分ける仕組み

ブランチは「作業ラインの分岐」です。

```
main ──●──●──●──●──●──●──  （本流）
              \          /
 feature ──────●──●──●──   （作業ブランチ）
```

- **main**: 本番ライン。常に動く状態を保つ
- **feature ブランチ**: 新しい作業用。完成したら main に合流（マージ）する

### よく使うブランチ操作

```bash
git branch                    # ブランチ一覧
git checkout -b feature/xxx   # 新しいブランチを作って切り替え
git checkout main              # main に戻る
```

> **ポイント**: ブランチを使うと「試しにやってみて、ダメだったら捨てる」が簡単にできます。main を壊す心配がありません。

## プルリクエスト（PR）— 変更をレビューしてもらう

プルリクエストは「この変更を main に取り込んでいいですか？」という提案です。

```bash
# ブランチで作業 → コミット → プッシュ → PR作成
gh pr create --title "レポートテンプレートを改善" --body "見出しの構成を変更しました"
```

GitHub CLI を使えば、ブラウザを開かずに PR を作れます。

### PR の流れ

```
1. ブランチを作る        git checkout -b feature/improve-template
2. 作業してコミット      git commit -m "テンプレート改善"
3. プッシュ             git push origin feature/improve-template
4. PR を作成            gh pr create
5. レビュー → マージ     （GitHub上で確認してマージ）
```

## Claude Code に頼むとき

| やりたいこと | Claude Code への頼み方 |
|------------|---------------------|
| GitHub にアップ | 「変更を push して」 |
| 最新を取得 | 「pull して最新にして」 |
| リポジトリ作成 | 「このプロジェクトを GitHub にリポジトリ作って push して」 |
| PR 作成 | 「この変更で PR を作って」 |
| ブランチ作成 | 「新しいブランチを作って」 |

Claude Code は `git` と `gh` コマンドを組み合わせて処理してくれます。

## 演習

→ `exercises/02-github/instructions.md` を開いて、演習に取り組みましょう
