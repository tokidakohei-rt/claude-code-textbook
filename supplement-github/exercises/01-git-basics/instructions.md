# 演習 1: Git の基本操作

## 課題

この演習では、このリポジトリを使って Git の基本操作を体験します。

### Task 1: 今の状態を確認する

以下のコマンドを実行して、リポジトリの状態を確認してください:

```bash
git status
```

表示された内容を読んで、以下を答えてください:
- 今いるブランチの名前は？
- 変更中のファイルはあるか？

### Task 2: 履歴を確認する

```bash
git log --oneline
```

以下を答えてください:
- コミットは何件あるか？
- 最新のコミットメッセージは何か？

### Task 3: ファイルを作ってコミットする

1. `supplement-github/` フォルダに `my-note.txt` というファイルを作成し、「Git の練習中」と書いてください

2. `git status` を実行して、ファイルが `Untracked files` に表示されることを確認

3. ファイルをステージングしてください:
   ```bash
   git add supplement-github/my-note.txt
   ```

4. `git status` を再実行して、`Changes to be committed` に変わったことを確認

5. コミットしてください:
   ```bash
   git commit -m "Git練習: my-note.txtを追加"
   ```

6. `git log --oneline` で新しいコミットが追加されたことを確認

### Task 4: Claude Code にコミットを頼む（チャレンジ）

1. `my-note.txt` の内容を「Git の練習完了！」に変更してください

2. 今度は自分でコマンドを打たず、Claude Code に頼んでください:
   ```
   my-note.txt の変更をコミットして
   ```

3. Claude Code がどんなコマンドを実行したか観察してください

### 後片付け

練習用ファイルを削除してコミットしましょう:

```
my-note.txt を削除してコミットして
```

## 確認ポイント

- [ ] `git status` で状態を確認できた
- [ ] `git log` で履歴を確認できた
- [ ] ファイルを作成し、add → commit の流れを体験した
- [ ] （チャレンジ）Claude Code にコミットを頼めた

完了したら「演習1終わりました」と伝えてください。
