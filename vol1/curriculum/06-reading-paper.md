# Chapter 6: 論文を読み解く（実践）

## ここからが本番

ここまでで学んだスキルを総動員して、論文を読み解きます。

## 論文の配置

`paper/` ディレクトリに論文ファイルを配置してください。

対応形式:
- **Markdown** (.md) — そのまま読める
- **PDF** (.pdf) — Claude Codeが直接読める（Readツール）
- **テキスト** (.txt) — そのまま読める

> サンプル論文として `paper/kahneman-tversky-1974.pdf`（Kahneman & Tversky, 1974「Judgment under Uncertainty: Heuristics and Biases」）が同梱されています。
> 自分の論文を使いたい場合は、`paper/` に別のファイルを置いてもOKです。

## 論文読解のステップ

### Step 1: 全体把握
まず論文の構造を把握します:
- タイトル・著者・年
- セクション構成（Introduction, Methods, Results, Discussion...）
- ページ数・図表の数

### Step 2: 要点抽出
各セクションから核心を抜き出します:
- **研究の問い**: 何を明らかにしようとしているか
- **手法**: どうやって調べたか
- **結果**: 何がわかったか
- **意義**: なぜ重要か
- **限界**: 何が足りないか

### Step 3: 構造化
スライドにするために情報を構造化します:
- 1スライド = 1メッセージ
- キーワードを抽出
- 図表で使える数値やデータを特定

## Claude Codeを使った効率的な読解

### Subagentを活用する

論文が長い場合、セクションごとにSubagentに分析させると効率的です:

```
「以下を並列で調べて:
  1. Introduction セクションの研究背景と目的
  2. Methods セクションの手法の概要
  3. Results セクションの主要な発見
  4. Discussion セクションの結論と限界」
```

### Skillを活用する

Chapter 4で作った `/report` Skillがここで活きます。
`/report paper/your-paper.pdf` で、決まったフォーマットのレポートを一発生成できます。

## 演習

→ `exercises/06-final-project/instructions.md` を開いて、最終プロジェクトに取り組みましょう
