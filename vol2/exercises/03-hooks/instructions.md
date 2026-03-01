# 演習 3: Hooks を設定してみる

## 課題

### Task 1: 現在の設定を確認する

`.claude/settings.local.json` の内容を読んで、現在どんな設定がされているか確認してください。

### Task 2: Hooks を追加する

`.claude/settings.local.json` に以下の Hook を追加してください:

**目標**: Write ツールでファイルが作成されるたびに、メッセージを表示する

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Write",
        "command": "echo '📝 新しいファイルが作成されました！'"
      }
    ]
  }
}
```

> **注意**: 既存の `permissions` の設定を消さないように気をつけてください。JSON の構造を壊さないように注意しましょう。

### Task 3: Hooks の動作を確認する

Hook を設定したら、実際にファイルを作成して動作を確認します:

```
output/hook-test.txt に「Hooks のテスト」と書いて
```

メッセージが表示されたら成功です。

### Task 4: オリジナル Hook を作る（チャレンジ）

自分なりの Hook を1つ追加してみましょう。

例:
- Bash コマンド実行前にログを記録する Hook
- Read ツール使用後にメッセージを表示する Hook

## 確認ポイント

- [ ] settings.local.json の構造を理解した
- [ ] PostToolUse Hook を追加できた
- [ ] Hook が実際に動作することを確認できた
- [ ] （チャレンジ）オリジナル Hook を作れた

完了したら「演習3終わりました」と伝えてください。
