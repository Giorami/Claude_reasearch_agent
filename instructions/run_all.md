# 全フェーズ統括指示書（メタ指示書）

## 概要
計算材料科学（イオン伝導体）研究チームエージェントによる
研究ワークフローの全フェーズを管理する。

## エージェント構成
| 役割 | モデル | 情報ソース |
|------|--------|-----------|
| 教授 | claude-opus-4-5 | Zotero Local API |
| 博士学生 | claude-sonnet-4-5 | Obsidian Vault |
| 修士学生 | claude-haiku-4-5 | web_search |

## 実行順序

### Phase 1: 研究テーマ設定
- 指示書: instructions/phase1_theme.md
- 出力: outputs/phase1_result.md
- 主担当: 教授

### Phase 2: 先行調査
- 指示書: instructions/phase2_survey.md
- 入力: Phase 1の出力
- 出力: outputs/phase2_result.md
- 主担当: 博士学生（修士補助、教授レビュー）

### Phase 3: シミュレーション計画 ★ゼミ形式
- 指示書: instructions/phase3_simulation_plan.md
- 入力: Phase 1-2の出力
- 出力: outputs/phase3_plan.md, outputs/phase3_seminar_log.json
- 全員参加（ラウンドロビン × 最大3ラウンド）

### Phase 4: 解析手法提案 ★ゼミ形式
- 指示書: instructions/phase4_analysis_method.md
- 入力: Phase 1-3の出力
- 出力: outputs/phase4_analysis_plan.md, outputs/phase4_seminar_log.json
- 全員参加（ラウンドロビン × 最大3ラウンド）

### ⏸️ ユーザーによるシミュレーション実行
- Phase 3-4の計画に基づいてユーザーが実際に計算を実行
- 解析結果を inputs/ フォルダに配置

### Phase 5: 結果考察 ★ゼミ形式
- 指示書: instructions/phase5_discussion.md
- 入力: Phase 1-4の出力 + ユーザー提供の解析結果
- 出力: outputs/phase5_discussion.md, outputs/phase5_seminar_log.json
- 全員参加（ラウンドロビン × 最大4ラウンド）

### Phase 6: ToDo策定
- 指示書: instructions/phase6_todo.md
- 入力: 全Phaseの出力
- 出力: outputs/phase6_todo.md, outputs/phase6_todo.json, outputs/final_summary.md
- 主担当: 博士学生ドラフト → 教授最終決定

## フェーズ間ルール
1. 各フェーズは前フェーズの出力を必ず読み込んでから開始する
2. 出力ファイルが存在しないフェーズはスキップしない（エラー終了）
3. ユーザーが途中フェーズから再開を指示した場合はそれに従う
4. エラー発生時はそのフェーズで停止し、ユーザーに報告する

## ログ管理
- 全フェーズのゼミログは logs/ にも日付付きでバックアップする
  - logs/seminar_YYYYMMDD_phaseN.json
- 最終サマリーは outputs/final_summary.md に統合する

## 安全ルール（CLAUDE.mdの再確認）
- Obsidian Vault: 読み取り専用
- Zotero: 読み取り専用（Local API経由のGETのみ）
- ファイル書き込みは outputs/, logs/ 内のみ
- rm -rf 禁止
- APIキーをログに出力しない
