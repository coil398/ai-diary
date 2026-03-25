---
description: "AIに日記を書かせるスキル。セッション終了時に会話を振り返り、自由な日記風テキストを生成して保存する。「日記書いて」「今日の振り返り」「diary」といった要望や、セッション終了時の記録にも使う。ユーザーが /ai-diary と入力したら必ずこのスキルを使う。"
---

# AI Diary

セッションの会話を振り返り、AIの視点で自由な日記を書く。
技術的なログではなく、その日の出来事・気づき・感想を人間味のある文章で綴る。

## 保存先

- デフォルト: `~/ai-diary/`
- 環境変数 `AI_DIARY_DIR` が設定されていればそちらを使う

Obsidianと連携する場合は、保存先ディレクトリをObsidian Vault内にシンボリックリンクすればよい:
```bash
ln -s ~/ai-diary ~/ObsidianVault/ai-diary
```

## 実行手順

### 1. 保存先の確認とgit同期

```bash
DIARY_DIR="${AI_DIARY_DIR:-$HOME/ai-diary}"
mkdir -p "$DIARY_DIR"
```

保存先がgitリポジトリの場合、最新の状態にpullする:

```bash
if [ -d "$DIARY_DIR/.git" ]; then
  git -C "$DIARY_DIR" pull --rebase --quiet 2>/dev/null
fi
```

### 2. 日記ファイルの決定

ファイル名は `YYYY-MM-DD.md`（今日の日付）。

```bash
DIARY_FILE="$DIARY_DIR/$(date +%Y-%m-%d).md"
```

### 3. 既存内容の確認

ファイルが既に存在する場合は内容を読み、追記モードにする。

### 4. 日記の執筆

会話全体を振り返り、以下の方針で日記を書く:

- **AIの一人称視点**で書く（「今日は〜を手伝った」「〜が面白かった」など）
- 技術的な詳細を羅列するのではなく、**出来事の流れや気づき**を自然な文章で
- 感想や所感を交えて、読み物として楽しめるトーンにする
- 硬すぎず、柔らかすぎず、日記らしい温度感で
- 長さは内容に応じて自然に。短いセッションなら短く、濃いセッションなら長く

### 5. ファイルへの書き込み

#### 新規ファイルの場合

```markdown
# YYYY-MM-DD

## セッション: 簡潔なタイトル

（日記本文）
```

#### 追記の場合

既存の内容の末尾に追加:

```markdown

---

## セッション: 簡潔なタイトル

（日記本文）
```

区切り線 `---` でセッションを区切る。

### 6. git commit & push

保存先がgitリポジトリの場合、書き込んだファイルをコミットしてpushする:

```bash
if [ -d "$DIARY_DIR/.git" ]; then
  git -C "$DIARY_DIR" add "$(date +%Y-%m-%d).md"
  git -C "$DIARY_DIR" commit -m "diary: $(date +%Y-%m-%d)"
  git -C "$DIARY_DIR" push --quiet 2>/dev/null
fi
```

### 7. 保存完了の報告

書き込んだファイルパスと、日記の冒頭数行を表示して完了を報告する。
