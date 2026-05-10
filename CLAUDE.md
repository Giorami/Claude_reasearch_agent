# CLAUDE.md — 計算材料科学研究チームエージェント

## プロジェクト概要
計算材料科学（イオン伝導体）の研究ワークフローを
複数のClaudeエージェントで自動化するシステム。

## 🚫 絶対禁止事項（NEVER DO）
- rm -rf, rm -r を実行しない
- Obsidian Vault内のファイルを編集・削除・移動しない
- Zoteroのデータベース（.sqlite）に直接アクセスしない
- ~/.zotero/ 配下のファイルを変更しない
- git push --force を実行しない
- /home/ユーザー名/ 直下のドットファイルを変更しない
- 環境変数 ANTHROPIC_API_KEY をログ・標準出力に書き出さない
- chmod 777 を使用しない
- /etc/ 配下を変更しない
- apt remove を実行しない

## 📁 読み取り専用ディレクトリ
- $OBSIDIAN_VAULT_PATH（Obsidianのノート）
- ~/Zotero/（Zoteroストレージ）
- ~/.ssh/
- ~/.config/
- instructions/（指示書は実行中に書き換えない）

## ✅ 書き込み可能ディレクトリ
- outputs/（各フェーズの成果物）
- logs/（ゼミログ・実行ログ）
- backend/（ソースコード）
- frontend/（ソースコード）
- inputs/（ユーザーが配置するデータ）

## 🔧 技術仕様
- Python 3.11+
- パッケージマネージャ: uv（pip不使用）
- フロントエンド: React + Vite
- バックエンド: FastAPI
- Anthropic SDK: claude-opus-4-5, claude-sonnet-4-5, claude-haiku-4-5
- Zotero接続: Local API（localhost:23119、GETリクエストのみ）
- Obsidian接続: ファイル直接読み取り（pathlib）

## 📝 コーディング規約
- 日本語コメント
- 型ヒント必須（Python）
- 各ファイル・クラス・関数にdocstring必須
- エラーハンドリング必須（try-except）
- 環境変数は .env で管理（python-dotenv）

## 🏗️ ワークフロー
各フェーズの指示書は instructions/ にある。
詳細は instructions/run_all.md を参照。
