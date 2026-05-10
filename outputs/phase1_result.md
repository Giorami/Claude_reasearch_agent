---
phase: 1
title: 研究テーマ設定レポート
agent: 教授エージェント（claude-opus-4-5）
date: 2026-05-11
sources: WebSearch（Zotero Local APIが無効のため補完利用）
---

# 研究テーマ設定レポート

> **データソースに関する注記**
> 本フェーズでは Zotero Local API（http://localhost:23119）への接続を試みたが、
> サーバから `"Local API is not enabled"` が返り、Zotero からの論文メタデータ取得は
> 行えなかった。指示書 `instructions/phase1_theme.md` の制約欄
> 「Zotero に十分な論文がない場合は web_search で補完してよいが、その旨を明記する」
> に従い、本レポートは **WebSearch による補完情報のみ** で構成されている。
> Phase 2（先行調査）では、ユーザに Zotero Local API（Edit → Preferences → Advanced
> → 「Allow other applications…」または対応する設定）の有効化を依頼することを推奨する。

---

## 1. 文献調査サマリー

直近2年（2024–2025）の計算材料科学・固体電解質研究を WebSearch（10 系統のクエリ）で
横断的に把握した。情報源は Nature/Nature Communications/npj Computational Materials/
JACS/Chem. Mater./ACS AMI/ACS Energy Lett./J. Mater. Chem. A/Adv. Energy Mater./
arXiv 等の査読論文・プレプリント計 30 編超。

主要な観察事項は以下のとおり。

1. **計算手法のパラダイム転換**：従来の DFT + AIMD（ナノ秒・百原子規模が上限）から、
   機械学習原子間ポテンシャル（MLIP）による「マイクロ秒・数千〜数万原子規模」MD への
   移行が決定的に進んだ。MatterSim・CHGNet・MACE・SevenNet・M3GNet・ORBFF 等の
   **ユニバーサル MLIP** が固体イオン伝導体に対しベンチマークされ、MatterSim が
   多くの指標で先行している（arXiv:2502.09970, 2025）。
2. **材料系の重心移動**：硫化物（Li6PS5Cl 等のアルジロダイト）への研究が依然厚いが、
   2023 年以降は **ハロゲン化物（Li3YCl6, Li3InCl6, Li3YCl6-xBrx 等）** が急速に台頭。
   高電圧安定性・乾燥環境耐性で硫化物に勝る一方、伝導機構の理解は発展途上。
3. **伝導機構研究の論争**：アニオン回転を介する「**paddle-wheel** 機構」を巡り、
   2024 年に Zhang ら（PNAS）が「paddle-wheel は存在しない、実体は **soft-cradle**
   （アニオン傾斜）である」とする反証論文を公表。Nature Comm. 2025 では
   NaTaCl6 で再び paddle-wheel が観測されたと報告。**機構の再定義は未決着の最前線**。
4. **粒界・界面の役割**：単結晶バルクの伝導度予測は MLIP-MD で実験と定量一致する
   レベルに到達したが、**多結晶バルクでは粒界が律速**となるケースが酸化物・硫化物
   ともに繰り返し報告。一方ハロゲン化物の粒界研究は手薄。
5. **AI/生成モデルの参入**：拡散生成モデルで非晶質 LiPO2F2 を発見、Active Learning と
   Fine-Tuning による低コスト構造探索（CHGNet 微調整で DFT 比 1/10000 のコスト）が
   2025 年に標準化されつつある。

---

## 2. 研究動向マップ

### 2.1 材料系別の動向

| 材料系 | 代表組成 | 最近 2 年の主要トピック | 体感的な論文密度 |
|--------|----------|------------------------|------------------|
| ガーネット型酸化物 | Li7La3Zr2O12 (LLZO) | 59 元素 177 試料の網羅的ドーピング探索（Anderson, Adv. Energy Mater. 2024）、Sb/Ga ドーピング、Deep Potential による T→C 相転移再現 | ★★★★（成熟期） |
| 硫化物アルジロダイト | Li6PS5Cl, Li6PS5Br | DPA-SSE 事前学習 Deep Potential（npj Comp. Mater. 2025）、25 ns × 6500 原子 MLIP-MD、Cl/S 配置乱雑さと伝導度の対応 | ★★★★（最も研究厚い） |
| ハロゲン化物 | Li3YCl6, Li3InCl6, Li3YCl6-xBrx, Li3ScCl6 | CHGNet 微調整による Y/Br 混合系の伝導度・構造予測（arXiv:2510.09861）、異方拡散、Li 金属界面分解 | ★★★（急成長中・空白多い） |
| NASICON 型酸化物 | Na3Zr2Si2PO12, Na4Hf2(SiO4)3 | S2− 置換でボトルネック拡張（ACS AEM 2025）、Sb ドーピングでデンドライト抑制 | ★★（Na 系で堅実に進展） |
| 反ペロブスカイト・closo-borate | Na3OCl 系, Li2B12H12 | paddle-wheel/soft-cradle 論争（PNAS 2024 vs Nat. Comm. 2025）、相転移と伝導度跳躍 | ★★（機構論で熱い） |
| クロライド超イオン伝導体 | NaTaCl6, NaNbCl6 | [TaCl6] 多面体回転と Na+ 伝導の結合、室温 3.3 mS/cm | ★★（新規・少数論文） |

### 2.2 計算手法別の動向

| 手法 | 役割 | 2025 年時点の到達点 |
|------|------|----------------------|
| DFT（PBE/HSE） | 静的構造・電子構造・形成エネルギー・電気化学窓 | 確立。スクリーニングのベースライン |
| AIMD（〜100 ps, 数百原子） | 高温外挿による拡散係数推定 | 確立。コストと統計精度がボトルネック |
| NEB / CI-NEB | 単一原子の遷移経路と活性化エネルギー | 確立。ただし協調拡散は捉えにくい |
| 古典 MD（Buckingham 等） | 粒界・大規模系 | 力場開発が律速、新材料には不適 |
| **MLIP（NNP, MTP, DeepPotential, MACE）** | ns–μs MD、大規模・多元素 | **主流化**。系特化学習で実験伝導度を再現 |
| **ユニバーサル MLIP（MatterSim, CHGNet, M3GNet, SevenNet, ORBFF）** | 学習なしで多系横断推論、Fine-Tuning で系特化 | **2025 年に Ready 宣言**（arXiv:2502.09970） |
| 拡散生成モデル | 構造生成・非晶質探索 | 2025 年に LiPO2F2 等で実例 |
| ベイズ最適化・能動学習 | 組成空間探索 | 標準ツール化 |

### 2.3 研究対象別の動向

| 対象 | 動向 |
|------|------|
| バルク伝導機構 | 単元素拡散から「協調拡散」「**paddle-wheel / soft-cradle**」へ。MLIP-MD が解析手段 |
| ドーピング効果 | 高スループット網羅（LLZO で 177 試料）と機構解明（Sb/Ga）が両輪 |
| **粒界** | 酸化物・硫化物で律速性を定量化、ハロゲン化物は空白 |
| 電極/電解質界面 | DFT による電気化学窓と CEI 組成（ROCO2Li, LiF, Li2CO3 等）の予測 |
| 相転移 | LLZO の T↔C、Li2B12H12 のオーダー/ディスオーダー転移、伝導度の数桁ジャンプ |
| 非晶質・乱雑性 | Li6PS5Cl の Cl/S 配置揺らぎと伝導度、非晶質 LiPO2F2 |

---

## 3. テーマ候補（3 件）

### 候補 1: ユニバーサル MLIP の Fine-Tuning によるハロゲン化物固体電解質 Li3M(Cl,Br,I)6 系の組成・アニオン混合設計

- **背景・動機**
  ハロゲン化物 SE は高電圧安定性と乾燥環境耐性で硫化物を上回るが、Y/In/Sc/Er/Ho 等の
  カチオンとハロゲンの混合空間は実験的に探索しきれていない。CHGNet 微調整による
  Li3YCl6-xBrx 予測（arXiv:2510.09861, 2025）が成功例として登場した直後で、
  **他のカチオン系・三元ハロゲン混合（Cl-Br-I）への一般化は未踏領域**。
- **新規性**
  - 単一系（Y/Cl-Br）に閉じた既報を、(Y, In, Sc) × (Cl, Br, I) の 9 端点 + 中間組成へ
    拡張する横断比較は前例なし。
  - MatterSim と CHGNet の Fine-Tuning 性能を同一プロトコルで比較した報告は
    ハロゲン化物では未発表。
- **実現可能性**
  - 必要計算資源：GPU 1–2 枚で MLIP 学習、CPU 数十コアで MD。研究室規模で十分。
  - ベースモデル（MatterSim, CHGNet）は公開済みで再現性が高い。
  - DFT 教師データは VASP/Quantum ESPRESSO で 数百〜数千点で足る（Fine-Tuning なので）。
- **期待されるインパクト**
  - 全固体電池のハロゲン化物カソードコート層・複合電解質設計に直結。
  - 「ユニバーサル MLIP は本当に Ready か」を実材料設計タスクで検証する重要な事例。
- **計算科学との親和性**
  - シミュレーションでしか網羅探索できない組成空間。MD トラジェクトリから
    伝導度・活性化エネルギー・異方性・構造安定性まで一貫して取得可能。

### 候補 2: ハロゲン化物多結晶固体電解質における粒界律速性の原子論的解明

- **背景・動機**
  酸化物（LLZO）・硫化物（Li6PS5Cl）の粒界律速性は定量化されたが、
  ハロゲン化物の粒界寄与は文献調査の範囲では実質的に空白。実用素子は
  必ず多結晶であり、ハロゲン化物の真の応用性能を決める因子。
- **新規性**
  - ハロゲン化物の粒界モデルを MLIP-MD で系統的に扱った先行研究は確認できず。
  - 「ソフトな格子（ハロゲン化物）の粒界 vs 硬い格子（酸化物）の粒界」の比較研究は
    本質的な学術的貢献になる。
- **実現可能性**
  - 多結晶モデル構築（Voronoi tessellation, 双結晶モデル）は確立手法。
  - ただし **1 万原子規模の長時間 MD** が必要で、計算コストは候補 1 より高い。
  - MLIP の粒界外挿性能の検証も並行して必要（リスク要因）。
- **期待されるインパクト**
  - バルク伝導度では硫化物に追いついたハロゲン化物が「実素子で勝てない理由」の
    解明に直結。粒界設計指針が出れば実験家への波及大。
- **計算科学との親和性**
  - 中性子回折等の実験では粒界の Li 分布を原子レベルで分解できない。
    シミュレーションでしか得られない知見が大きい。

### 候補 3: paddle-wheel vs soft-cradle 論争に対する MLIP-MD による機構判別フレームワークの構築

- **背景・動機**
  Zhang ら（PNAS 2024）の「paddle-wheel は存在しない、実体は soft-cradle」とする
  主張と、それ以降も paddle-wheel を主張する Nature 系論文（NaTaCl6, 2025）が
  並走しており、機構そのものの定義が分野で揺らいでいる。**判別のための
  定量的・操作的指標が確立されていない**ことが論争の温床。
- **新規性**
  - アニオン回転自己相関関数と Li/Na 拡散の交差相関を統一プロトコルで複数系
    （Li2B12H12, NaTaCl6, Na3PS4, Li6PS5Cl 等）に適用する横断研究は未報告。
  - 「機構の定義」自体に貢献できる稀少なテーマ。
- **実現可能性**
  - 既存の MLIP（DPA-SSE 等）と公開構造を組み合わせ MD を回すだけで主要解析が可能。
  - **計算コストは候補 1〜2 より低い**が、機構の操作的定義の妥当性を
    査読プロセスで通すには議論力が必要（理論的リスク）。
- **期待されるインパクト**
  - 機構論への寄与は応用設計にも波及（どの自由度を「狙えば」伝導度が上がるか）。
  - 学術的価値は最も高いが、応用インパクトは候補 1 より間接的。
- **計算科学との親和性**
  - アニオン回転と並進の結合は実験では分離測定が極めて困難。MD が唯一の手段。

---

## 4. 選定テーマ

### テーマ名
**「ユニバーサル機械学習原子間ポテンシャルの Fine-Tuning に基づく
ハロゲン化物固体電解質 Li3M(Cl,Br,I)6（M = Y, In, Sc）の組成設計と伝導機構解明」**

### 選定理由
3 候補を以下の評価軸で比較した：

| 評価軸 | 候補 1（混合ハロゲン化物設計） | 候補 2（粒界律速性） | 候補 3（機構判別） |
|--------|--------------------------------|----------------------|--------------------|
| 新規性 | ◎ 三元混合空間は未踏 | ◯ ハロゲン化物粒界は空白 | ◎ 機構定義への貢献 |
| 実現可能性 | ◎ 公開 MLIP で即着手可 | △ 1 万原子 MD は重い | ◎ コスト低 |
| インパクト | ◎ 全固体電池設計に直結 | ◯ 実素子性能の鍵 | △ 応用は間接的 |
| 計算科学親和性 | ◎ 探索 × 機構の両得 | ◯ メソスケール特化 | ◎ MD でしか出ない知見 |
| 中間成果の出しやすさ | ◎ 端点組成から段階的 | △ モデル構築に時間 | ◯ 系横断で並列化可 |
| 半年〜1 年でのまとまり | ◎ | △ | ◯ |

**候補 1 を選定**する。理由：

1. **新規性・実現可能性・インパクト・親和性の 4 軸すべてで上位**。
2. 候補 2 は計算コストと MLIP 外挿性能のリスクが大きく、初動の研究室では
   失敗時のリカバリが効きにくい。
3. 候補 3 は学術価値が高いが、応用インパクトと中間成果の物理量が抽象的で、
   修士・博士の学位研究としてはアウトプットが見えにくい。
4. ただし候補 1 の枠組みの中で **代表組成について候補 3 的な機構解析（アニオン
   回転と Li 並進の結合解析）を内包**することで、機構論的貢献も同時に狙う。
5. 候補 2 は将来の発展テーマとして温存する（多結晶モデルへの拡張）。

### 研究目標（最終ゴール）
公開ユニバーサル MLIP（MatterSim および CHGNet）の Fine-Tuning によって
**(Y, In, Sc) × (Cl, Br, I)** の 9 端点と代表的中間組成（合計 20–30 組成）の
**室温イオン伝導度・活性化エネルギー・拡散異方性・構造安定性** を予測し、
全固体電池応用に向けた最有望組成 3 件を同定する。
さらに代表 1–2 組成については AIMD による検証と、アニオン副格子ダイナミクスと
Li+ 並進の相関解析を行い、伝導機構（協調拡散 / paddle-wheel / soft-cradle）を
評価する。

### 中間マイルストーン

| # | マイルストーン | 想定期間 | 主成果物 |
|---|---------------|----------|----------|
| M1 | 既知系（Li3YCl6, Li3YBr6, Li3InCl6）での MLIP 再現性検証 | 1 か月 | Fine-Tuning パイプライン、伝導度の文献再現 |
| M2 | 9 端点組成のフルスクリーニング（MD: ns 規模） | 2 か月 | σ(T), Ea, 異方性のデータベース |
| M3 | 中間組成・カチオン混合系の探索（10–20 組成） | 2 か月 | 有望組成 3 件の同定 |
| M4 | 有望組成の AIMD 検証 + 機構解析（回転-並進結合） | 2 か月 | 機構論的考察、論文ドラフト初稿 |
| M5 | 実験家との比較・限界の整理・粒界拡張の方針策定 | 1 か月 | 最終レポート、Phase 6 ToDo への接続 |

---

## 5. 次フェーズへの申し送り事項

Phase 2（先行調査）に向けて以下を引き継ぐ。

### Phase 2 で深掘りすべき論文・トピック

1. **必読論文（直接的に方法論を流用）**
   - Predicting Crystal Structures and Ionic Conductivity in Li3YCl6-xBrx … Fine-Tuned MLIP
     （arXiv:2510.09861, 2025）—— 本テーマの最も近い先行研究。手法・データ・結果を完全に把握する。
   - Universal Machine Learning Interatomic Potentials are Ready for Solid Ion Conductors
     （arXiv:2502.09970, 2025）—— ユニバーサル MLIP のベンチマーク。MatterSim 採用の根拠。
   - DPA-SSE: A pre-trained deep potential model for sulfide solid electrolytes
     （npj Comp. Mater. 2025）—— 事前学習モデルの構築哲学を参照。
2. **比較対象（背景知識として必須）**
   - LLZO のドーピング網羅探索（Adv. Energy Mater. 2024, Anderson ら）—— 探索手法の比較。
   - Li6PS5Cl の MLIP-MD（ACS AMI 2024, 25 ns × 6500 原子）—— 統計精度の参照値。
   - Li3YCl6 異方拡散研究（ACS AEM 2024）—— 異方性解析の手法。
3. **機構論の理解（最終フェーズに効く）**
   - paddle-wheel レビュー（Nat. Rev. Mater. 2021）と反証論文（PNAS 2024）の両方。
   - NaTaCl6 paddle-wheel 論文（Nat. Comm. 2025）。

### Phase 2 で確認すべき技術的事項

- MatterSim / CHGNet の最新公開バージョンとライセンス。
- Fine-Tuning に必要な DFT 計算量（教師データ点数の目安、PBE/PBEsol どちらか）。
- Li3M(Cl,Br,I)6 系の実験的結晶構造（ICSD/Materials Project エントリの有無）。
- 中性子回折・NMR・インピーダンス実験との比較に使える文献値の整理。

### Phase 3 シミュレーション計画に向けた未決事項

- 計算規模：1 組成あたり何原子・何 ns 必要か（M1 で確定）。
- 温度範囲：Arrhenius 外挿の妥当性検証温度（300–800 K 推奨）。
- 統計：複数初期配置の本数（5–10 本推奨）。
- 検証指標：MSD, ハール VAR, 非ガウス係数 α2, 回転自己相関（アニオン）。

### Phase 1 で残った課題（環境整備）

- **Zotero Local API の有効化をユーザに依頼**。Phase 2 以降の文献収集を
  Zotero ベースに切り替えることで、論文管理の一貫性と再現性が向上する。
- 接続先：`http://localhost:23119/api/users/0/...`
- Zotero 設定：Edit → Preferences → Advanced → Allow other applications を有効化。

---

## 付録: 参照した主要 Web ソース

本レポートは以下の WebSearch 結果に基づく（Zotero 不使用のため列挙）。

- LLZO 包括的ドーパント探索: Anderson et al., Adv. Energy Mater., 2024
  https://advanced.onlinelibrary.wiley.com/doi/full/10.1002/aenm.202304025
- LLZO・固体電解質計算レビュー: PMC11820664（2025）
  https://pmc.ncbi.nlm.nih.gov/articles/PMC11820664/
- AI agents for solid electrolytes（2025）
  https://www.oaepublish.com/articles/aiagent.2025.10
- DPA-SSE 事前学習 Deep Potential（npj Comp. Mater., 2025）
  https://www.nature.com/articles/s41524-025-01764-6
- Li6PS5Cl の MLIP-MD（ACS AMI, 2024）
  https://pubs.acs.org/doi/10.1021/acsami.4c08865
- Argyrodite MTP MD（J. Materiomics, 2024）
  https://www.sciencedirect.com/science/article/abs/pii/S2211285524001848
- Li3YCl6-xBrx Fine-Tuned MLIP（arXiv:2510.09861, 2025）
  https://arxiv.org/html/2510.09861
- Universal MLIPs are Ready for SICs（arXiv:2502.09970, 2025）
  https://arxiv.org/abs/2502.09970
- Li3YCl6 異方拡散（ACS AEM, 2024）
  https://pubs.acs.org/doi/10.1021/acsaem.4c01249
- ハロゲン化物 SE レビュー（Energy Mater. Adv., 2025）
  https://spj.science.org/doi/10.34133/energymatadv.0092
- NASICON Na4Hf2(SiO4)3 ML+DFT（ACS Omega, 2025）
  https://pubs.acs.org/doi/10.1021/acsomega.5c11236
- NASICON S 置換設計（ACS AEM, 2025）
  https://pubs.acs.org/doi/10.1021/acsaem.5c02732
- NASICON 電解質レビュー（Carbon Energy, 2025）
  https://onlinelibrary.wiley.com/doi/10.1002/cey2.70031
- Li6.75La3Zr1.5Ta0.5O12 粒界拡散（2022/23）
  https://pubmed.ncbi.nlm.nih.gov/36330867/
- Li6PS5Cl 粒界 MLIP-MD（arXiv:2407.04126, 2024）
  https://arxiv.org/html/2407.04126v2
- 多結晶反ペロブスカイト粒界（J. Phys. Chem. C, 2020）
  https://pubs.acs.org/doi/10.1021/acs.jpcc.0c07328
- NaTaCl6 paddle-wheel（Nat. Comm., 2025）
  https://www.nature.com/articles/s41467-025-61738-6
- paddle-wheel 設計レビュー（Nat. Rev. Mater., 2021）
  https://www.nature.com/articles/s41578-021-00401-0
- paddle-wheel 反証（PNAS, 2024）
  https://www.pnas.org/doi/10.1073/pnas.2316493121
- 固体電解質界面安定性レビュー（Adv. Sci., 2025）
  https://advanced.onlinelibrary.wiley.com/doi/10.1002/advs.202517032

（URL は WebSearch 結果より取得。本リストはユーザが提示した情報ではなく、
プログラミング補助のための文献参照として記載した。）
