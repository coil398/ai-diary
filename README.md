# ai-diary

Claude Code のスキルとして動作する、AIが自動で日記を書くツール。

セッション中の会話を振り返り、AIの一人称視点で日記風のテキストを生成・保存する。技術ログではなく、出来事の流れや気づき・感想を自然な文章で綴る。

## インストール

Claude Code のスキルとしてこのリポジトリをクローンする:

```bash
# ユーザースコープ（全プロジェクト共通）
git clone https://github.com/coil398/ai-diary.git ~/.claude/skills/ai-diary

# プロジェクトスコープ（特定プロジェクトのみ）
git clone https://github.com/coil398/ai-diary.git .claude/skills/ai-diary
```

## 使い方

Claude Code のセッション中に以下のように呼び出す:

```
/ai-diary
```

「日記書いて」「今日の振り返り」などの自然言語でも起動する。

## 保存先

- デフォルト: `~/ai-diary/`
- 環境変数 `AI_DIARY_DIR` で変更可能

## Obsidian 連携

保存先ディレクトリを Obsidian Vault 内にシンボリックリンクすれば、Obsidian から日記を閲覧・管理できる:

```bash
ln -s ~/ai-diary ~/ObsidianVault/ai-diary
```

## Git 同期

保存先が git リポジトリ内にある場合、日記の書き込み前に `pull`、書き込み後に `commit` & `push` を自動で行う。

## 出力フォーマット

ファイル名は `YYYY-MM-DD.md`。同日に複数セッションがあれば `---` 区切りで追記される。

```markdown
# 2025-03-25

## セッション: リファクタリング祭り（14:30）

今日はコードの大掃除を手伝った。...

---

## セッション: バグ退治（18:00）

夕方から厄介なバグと格闘した。...
```

## ライセンス

MIT
