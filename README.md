# 🔬 計算材料科学 研究チームエージェント

## 概要

計算材料科学（イオン伝導体）の研究ワークフローを、複数のClaudeエージェントで自動化するシステムです。
異なる能力レベルのAIモデルに「教授」「博士学生」「修士学生」の役割を持たせ、
実際の研究室のゼミのように議論・ブラッシュアップを行いながら研究を進めます。

---

## このワークフローでできること

### 🎯 研究テーマの設定
教授エージェントがZoteroの論文データベースを参照し、
研究動向を分析した上で、新規性・実現可能性・インパクトの観点から
最適な研究テーマを提案します。

### 📚 体系的な先行調査
博士学生がObsidianの研究ノートを活用し、修士学生がWeb検索で最新論文を収集。
教授がレビューすることで、抜け漏れのない先行研究マップが作成されます。

### 💬 ゼミ形式のディスカッション
修士学生→博士学生→教授の順にラウンドロビンで議論。
修士学生のフレッシュなアイデアを、博士学生が技術的に検証し、
教授が俯瞰的に評価・方向修正する——実際の研究室ゼミを再現します。

### 🖥️ シミュレーション計画の策定
対象材料、計算手法（DFT/AIMD/ML-MD）、計算条件（カットオフ、k点メッシュ、温度等）を
議論を通じて具体的に決定します。

### 📊 解析手法の設計
MSD、RDF、活性化エネルギー計算など、シミュレーション結果の解析方法を
事前に計画します。統計処理・誤差評価の方法も含みます。

### 🔍 結果の考察
実際のシミュレーション結果をエージェントチームが多角的に考察。
先行研究との比較、物理的解釈、論文ストーリーラインの提案まで行います。

### ✅ ToDo策定
議論の結果を踏まえた具体的なアクションアイテムを、
担当者・優先度・期限付きで作成します。

---

## エージェント構成

| 役割 | モデル | 情報ソース | 特徴 |
|------|--------|-----------|------|
| 👨‍🏫 教授 | claude-opus-4-5 | Zotero（論文DB） | 俯瞰的判断・最終決定・厳密な評価 |
| 🎓 博士学生 | claude-sonnet-4-5 | Obsidian Vault（研究ノート） | 技術的検証・批判的思考・詳細設計 |
| 📚 修士学生 | claude-haiku-4-5 | Web検索（arXiv等） | アイデア出し・素朴な疑問・最新トレンド |

---

## フェーズ構成

```
Phase 1  研究テーマ設定      ← 教授主導
Phase 2  先行調査            ← 博士主導（修士補助、教授レビュー）
Phase 3  シミュレーション計画 ← ★ゼミ形式ディスカッション
Phase 4  解析手法提案        ← ★ゼミ形式ディスカッション
         ⏸️ ユーザーがシミュレーション実行
Phase 5  結果考察            ← ★ゼミ形式ディスカッション
Phase 6  ToDo策定            ← 博士ドラフト → 教授最終決定
```

---

## 使い方

### 動作環境

- Claude Code（Anthropic CLI）
- Anthropic API Key
- Python 3.11+（WebUI版のみ）
- Node.js 18+（WebUI版のみ）
- Zotero（オプション：論文管理ソフト）
- Obsidian（オプション：研究ノート）

### 方法1：指示書バージョン（ターミナルで使う）

Claude Codeに指示書を読ませて各フェーズを実行する方法です。
WebUI不要ですぐに使い始められます。

#### 初回セットアップ

```bash
# 1. プロジェクトフォルダに移動
cd ~/your-project-folder/

# 2. 必要なフォルダが揃っていることを確認
ls instructions/   # phase1〜6 の指示書 + run_all.md
ls CLAUDE.md       # 安全ルール・設定

# 3. outputs と logs フォルダを作成
mkdir -p outputs logs inputs

# 4. CLAUDE.md を編集して自分の環境に合わせる
#    - Obsidian Vault のパス
#    - Zotero の設定

# 5. Claude Code を起動
claude
```

#### Phase 1：研究テーマ設定

```
あなた → Claude Code:

instructions/phase1_theme.md を読んで、その内容に従って実行して。
```

実行後、`outputs/phase1_result.md` が生成されます。内容を確認します。

```
あなた → Claude Code:

outputs/phase1_result.md を見せて。
```

修正が必要な場合：

```
あなた → Claude Code:

テーマ候補2の方が面白いので、そちらを選定テーマに変更して。
理由も書き直して。
```

#### Phase 2：先行調査

```
あなた → Claude Code:

outputs/phase1_result.md が完了しているので、
instructions/phase2_survey.md を読んで Phase 2 を実行して。
```

#### Phase 3：シミュレーション計画（ゼミ形式）

```
あなた → Claude Code:

instructions/phase3_simulation_plan.md を読んでゼミを開始して。
Phase 1-2 の出力を入力として使うこと。
```

ゼミ進行中に介入したい場合：

```
あなた → Claude Code:

修士学生の提案にあったGNNポテンシャルの方向は面白いので、
そこを深掘りする方向でラウンド2を進めて。
```

#### Phase 4：解析手法提案（ゼミ形式）

```
あなた → Claude Code:

instructions/phase4_analysis_method.md を読んでゼミを開始して。
Phase 1-3 の出力を入力として使うこと。
```

#### ⏸️ シミュレーション実行（ユーザー作業）

Phase 3-4で策定した計画に基づいて、ユーザーが実際にシミュレーション
（VASP, LAMMPS等）を実行します。解析結果を `inputs/` に配置します。

```bash
# 例
cp ~/simulation_results/msd_results.csv inputs/
cp ~/simulation_results/rdf_data.csv inputs/
cp ~/simulation_results/trajectory.xyz inputs/
```

#### Phase 5：結果考察（ゼミ形式）

```
あなた → Claude Code:

inputs/ フォルダにシミュレーション結果を配置した。
以下のファイルがある：
- inputs/msd_results.csv
- inputs/rdf_data.csv
- inputs/trajectory.xyz
instructions/phase5_discussion.md を読んで Phase 5 を実行して。
```

シミュレーション結果がまだない場合でも、仮想的な議論ができます：

```
あなた → Claude Code:

まだシミュレーション結果はないが、
Phase 3-4の計画に基づいて「こういう結果が得られた場合」の
仮想的な考察をゼミ形式で行って。
instructions/phase5_discussion.md を参照して。
```

#### Phase 6：ToDo策定

```
あなた → Claude Code:

instructions/phase6_todo.md を読んで最終ToDoを作成して。
```

#### 全フェーズ連続実行（上級者向け）

```
あなた → Claude Code:

instructions/run_all.md を読んで、Phase 1 から Phase 4 まで順に実行して。
各フェーズの出力を次のフェーズの入力として使うこと。
```

#### 途中から再開する場合

```
あなた → Claude Code:

前回 Phase 2 まで完了している。
outputs/ に phase1_result.md と phase2_result.md がある。
instructions/phase3_simulation_plan.md を読んで Phase 3 から再開して。
```

#### 便利なコマンド

```
# 特定のエージェントだけ使う
博士学生エージェントだけ使って、
Obsidian Vault 内の LLZO 関連ノートを全部リストアップして。

# ゼミの追加ラウンド
Phase 3 のゼミの議論が浅い。
もう1ラウンド追加して、特に温度条件の設定について深く議論させて。

# 指示書の修正
instructions/phase3_simulation_plan.md を修正して。
ディスカッションの論点に「機械学習ポテンシャルの訓練データ設計」を追加して。

# Zotero/Obsidian の接続確認
Zotero API に接続テストして。localhost:23119 にアクセスできるか確認して。
Obsidian Vault のパスが読み取れるか確認して。
```

---

### 方法2：WebUIバージョン（ブラウザで使う）

指示書バージョンで動作確認後、WebUIを構築してブラウザから操作する方法です。

#### WebUI開発手順

`webui_instructions/` フォルダに4段階の開発指示書があります。

```
Step 1: webui_instructions/step1_backend_skeleton.md
        → FastAPIバックエンドの骨格

Step 2: webui_instructions/step2_agent_engine.md
        → エージェントエンジン・Zotero/Obsidian連携

Step 3: webui_instructions/step3_frontend.md
        → React フロントエンド

Step 4: webui_instructions/step4_integration.md
        → 統合テスト・仕上げ
```

各StepをClaude Codeに読ませて順番に実行します：

```
あなた → Claude Code:

webui_instructions/step1_backend_skeleton.md を読んで実行して。
```

詳細は `webui_instructions/README.md` を参照してください。

#### WebUI起動方法（開発完了後）

```bash
# ターミナル1: バックエンド
cd backend
uvicorn main:app --reload --port 8000

# ターミナル2: フロントエンド
cd frontend
npm run dev

# ブラウザで http://localhost:5173 にアクセス
```

---

## フォルダ構成

```
your-project-folder/
├── CLAUDE.md                     # 安全ルール・プロジェクト設定
├── README.md                     # このファイル
├── .env                          # 環境変数（APIキー等）
│
├── instructions/                 # フェーズ別指示書（方法1で使用）
│   ├── run_all.md                #   全フェーズ統括
│   ├── phase1_theme.md           #   テーマ設定
│   ├── phase2_survey.md          #   先行調査
│   ├── phase3_simulation_plan.md #   シミュレーション計画
│   ├── phase4_analysis_method.md #   解析手法提案
│   ├── phase5_discussion.md      #   結果考察
│   └── phase6_todo.md            #   ToDo策定
│
├── webui_instructions/           # WebUI開発指示書（方法2で使用）
│   ├── README.md                 #   開発手順ガイド
│   ├── step1_backend_skeleton.md #   バックエンド骨格
│   ├── step2_agent_engine.md     #   エージェントエンジン
│   ├── step3_frontend.md         #   フロントエンド
│   └── step4_integration.md      #   統合テスト
│
├── inputs/                       # ユーザーのシミュレーション結果
├── outputs/                      # 各フェーズの成果物
│   ├── phase1_result.md
│   ├── phase2_result.md
│   ├── phase3_plan.md
│   ├── phase3_seminar_log.json
│   ├── phase4_analysis_plan.md
│   ├── phase4_seminar_log.json
│   ├── phase5_discussion.md
│   ├── phase5_seminar_log.json
│   ├── phase6_todo.md
│   ├── phase6_todo.json
│   └── final_summary.md
├── logs/                         # ゼミログのバックアップ
│
├── backend/                      # WebUIバックエンド（方法2）
└── frontend/                     # WebUIフロントエンド（方法2）
```

---

## Zotero / Obsidian の設定

### Zotero Local API（オプション）

教授エージェントがZoteroの論文データベースにアクセスします。
未接続の場合はWeb検索にフォールバックします。

```
Zotero アプリを起動
→ 編集 → 設定 → 詳細
→ 「他のアプリケーションからのリクエストを許可する」にチェック
→ ポート: 23119
```

確認方法：
```bash
curl http://localhost:23119/api/users/0/items?limit=1
```

### Obsidian Vault（オプション）

博士学生エージェントがObsidianの研究ノートを参照します。
未接続の場合はWeb検索にフォールバックします。

CLAUDE.md または .env にVaultのパスを設定してください：
```
OBSIDIAN_VAULT_PATH=/home/username/your-vault
```

Vault内の推奨構造：
```
your-vault/
├── papers/          # 論文要約ノート
│   ├── LLZO_review.md
│   └── AIMD_methods.md
├── methods/         # 手法メモ
├── ideas/           # アイデアメモ
└── daily/           # デイリーノート
```

ノートに `#ion-conductor` `#DFT` `#AIMD` 等のタグを付けると
検索精度が向上します。

---

## 安全設計

### 読み取り専用の保護

| 対象 | 保護方法 |
|------|---------|
| Obsidian Vault | 読み取りのみ。書き込み・削除禁止 |
| Zotero データベース | Local API経由のGETリクエストのみ |
| instructions/ | 実行中は変更しない |

### 禁止操作

以下の操作はCLAUDE.mdで明示的に禁止されています：
- `rm -rf` / `rm -r`（ファイル削除）
- Obsidian/Zoteroファイルの編集・削除
- `git push --force`
- APIキーのログ出力
- システムファイルの変更

### 推奨：OSレベルの保護

```bash
# Obsidian Vault を読み取り専用にする
chmod -R a-w ~/your-vault/

# Zotero データを読み取り専用にする
chmod -R a-w ~/Zotero/
```

---

## カスタマイズ

### 研究分野の変更

指示書のシステムプロンプトを書き換えることで、
イオン伝導体以外の分野にも対応できます：
- 触媒材料
- 半導体
- 磁性材料
- 高分子
- etc.

### エージェント構成の変更

CLAUDE.md と指示書のモデル指定を変更すれば、
エージェントの能力配分を調整できます：
- 全員Sonnetにしてコスト削減
- 教授をOpusにして判断の質を上げる
- 学部生を追加して4人体制にする

### ゼミのラウンド数変更

各指示書の「最大Nラウンド」を変更するか、
Claude Codeへの指示時に指定します：

```
Phase 3 のゼミを5ラウンドで実行して。
```

---

## ライセンス

このプロジェクトはAnthropicのAPIを使用しています。
利用にはAnthropicの利用規約に従ってください。
