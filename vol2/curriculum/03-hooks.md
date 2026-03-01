# Chapter 3: Hooks - 自動化トリガーを設定する

## Hooks とは？

Hooksは、Claude Codeのツール実行の **前後に自動で走る処理** を設定する仕組みです。

たとえば:
- ファイルを書き込んだ **後に** 自動で通知する
- Bashコマンドを実行する **前に** ログを記録する
- Claude Codeが処理を終える **たびに** 何かを実行する

Vol.1 で学んだ Rules（CLAUDE.md）が「AIの振る舞いを制御する」のに対し、Hooks は **「AIの行動に連動してシステムを動かす」** ものです。

## Hooks の種類

| Hook | タイミング | 用途の例 |
|------|-----------|---------|
| PreToolUse | ツール実行の **前** | 危険な操作の確認、ログ記録 |
| PostToolUse | ツール実行の **後** | 通知、後処理 |
| Notification | 通知が発生したとき | 外部への連携 |
| Stop | Claude Code が応答を終えるとき | 最終チェック、レポート生成 |

## Hooks の設定方法

Hooks は `.claude/settings.local.json` に記述します。

### 例: ファイル書き込み後に通知する

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Write",
        "command": "echo '📝 ファイルが作成されました'"
      }
    ]
  }
}
```

この設定を入れると、Claude Code が Write ツールでファイルを作成するたびに、ターミナルにメッセージが表示されます。

### 設定の構造

```json
{
  "hooks": {
    "[Hook種類]": [
      {
        "matcher": "[対象ツール名]",
        "command": "[実行するシェルコマンド]"
      }
    ]
  }
}
```

- **matcher**: どのツールに反応するか（`Write`, `Bash`, `Read` など。`*` で全ツール）
- **command**: 実行するシェルコマンド（ターミナルで動くコマンドならなんでも）

## 実用的な Hooks の例

### 例1: output/ に書き込みがあったらメッセージを表示

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Write",
        "command": "echo '✅ output/ ディレクトリを確認してください'"
      }
    ]
  }
}
```

### 例2: Bash 実行前にログを記録

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "command": "echo \"[$(date)] Bash command executed\" >> .claude/hook-log.txt"
      }
    ]
  }
}
```

### 例3: 処理完了時にサマリーを促す

```json
{
  "hooks": {
    "Stop": [
      {
        "command": "echo '🏁 処理が完了しました。/review で成果物を確認しましょう'"
      }
    ]
  }
}
```

## Hooks を使うときの注意

1. **シンプルに保つ**: Hooks は補助的な仕組み。複雑なロジックは入れない
2. **失敗しても止まらないようにする**: Hooks のコマンドがエラーでもClaude Code本体は動き続ける
3. **既存の設定を壊さない**: `settings.local.json` を編集するときは、既存の `permissions` 等を消さないように注意

## Rules / Skills / Hooks の比較

| 仕組み | 何を制御するか | 設定場所 |
|-------|--------------|---------|
| Rules (CLAUDE.md) | AIの考え方・振る舞い | `CLAUDE.md` |
| Skills | AIに実行させる定型処理 | `.claude/commands/` |
| Hooks | ツール実行に連動する自動処理 | `.claude/settings.local.json` |

> **ポイント**: Rules は「AIへの指示」、Skills は「AIが使うテンプレート」、Hooks は「システムレベルの自動化」。3つを組み合わせることで、Claude Code の動作を細かくカスタマイズできます。

## 演習

→ `exercises/03-hooks/instructions.md` を開いて、演習に取り組みましょう
