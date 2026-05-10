---
phase: 2
title: 先行調査レポート（Phase 3 動作確認用ダミー）
agent: 博士学生エージェント（claude-sonnet-4-5）／補助：修士学生・教授
date: 2026-05-11
status: PLACEHOLDER（Phase 3 動作確認のため最小構成で作成）
---

# 先行調査レポート（ダミー）

> **注記**: 本ファイルは Phase 3 のゼミ模擬実行の入力として最小限の体裁で
> 作成されたプレースホルダである。実運用では `instructions/phase2_survey.md`
> に従い、Zotero / Obsidian / WebSearch を組み合わせた本格的な先行調査結果を
> 配置する必要がある。Phase 1 で抽出した必読論文を起点とする。

## 1. 調査対象テーマ
ユニバーサル機械学習原子間ポテンシャル（MLIP）の Fine-Tuning による
ハロゲン化物固体電解質 Li3M(Cl,Br,I)6（M = Y, In, Sc）の組成設計と伝導機構解明。

## 2. 必読論文サマリー

### 2.1 直接的先行研究
- **arXiv:2510.09861 (2025)**: CHGNet 微調整による Li3YCl6-xBrx の構造・伝導度予測。
  教師データ ~2000 点、PBE+D3、6×6×6 スーパーセル相当、300–600 K MD（各 5 ns × 5 本）。
  σ_RT 予測誤差は実験比 0.3 オーダー以内。**本テーマの直接的雛形**。
- **arXiv:2502.09970 (2025)**: ユニバーサル MLIP ベンチマーク。
  MatterSim が Li6PS5Cl, LLZO, Li3YCl6 を含む 12 系統で MAE 最小。
  Fine-Tuning なしでも定性傾向は再現、Ea 絶対値は 10–20% 誤差。
- **npj Comp. Mater. 2025 (DPA-SSE)**: 硫化物特化事前学習モデル構築の哲学。
  Active Learning ループで教師データ ~5000 点に抑制。

### 2.2 計算条件の参照
- **ACS AMI 2024 (Li6PS5Cl MLIP-MD)**: 25 ns × 6500 原子、NVT、300–800 K、
  300 K で σ ≈ 4 mS/cm（実験 ~3 mS/cm）。MSD は 5 ns 以降線形領域到達。
- **ACS AEM 2024 (Li3YCl6 異方拡散)**: 異方性比 σ_ab/σ_c ≈ 5–10。
  AIMD 100 ps × 5 温度、PBE、500 eV cutoff、Γ-only。

### 2.3 機構論
- **PNAS 2024 (Zhang)**: paddle-wheel 否定、soft-cradle 提唱。
- **Nat. Comm. 2025 (NaTaCl6)**: paddle-wheel 再観測。判別指標は未統一。

## 3. 技術的論点の整理

| 論点 | 推奨範囲 | 根拠 |
|------|----------|------|
| MLIP ベース | MatterSim or CHGNet | arXiv:2502.09970, 2510.09861 |
| DFT 汎関数 | PBE+D3 | ハロゲン化物標準 |
| エネルギーカットオフ | 520 eV 以上 | VASP Materials Project 標準 |
| k点メッシュ（教師 DFT） | 2×2×2（スーパーセル） | 2510.09861 準拠 |
| MD アンサンブル | NVT（Nosé-Hoover） | 既報踏襲 |
| MD 時間 | 5 ns × 5–10 本 / 温度 | 統計収束 |
| 温度範囲 | 300, 400, 500, 600, 800 K | Arrhenius 5 点 |
| スーパーセル | 1000–2000 原子 | 粒界なしバルク |

## 4. 未決事項（Phase 3 で議論）
- MatterSim と CHGNet どちらを主軸にするか（両方並走か）
- AIMD 検証の規模・温度
- アニオン回転自己相関の解析プロトコル
- 計算資源見積もり（GPU 時間）
- 9 端点 + 中間組成の優先順位

## 5. Phase 3 への引き継ぎ
Phase 3 では本レポートに基づき、修士学生（新規手法提案）、
博士学生（パラメータ精緻化）、教授（妥当性評価）の三者で
ラウンドロビン議論を行い、シミュレーション計画を確定する。
