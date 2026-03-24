# BRA & BCM：Gemini 對話筆記

**來源：** Gemini 對話（2026/3/13 – 3/20）
**主題：** BRA 論文價值評估、BCM 治理框架、論文章節結構、待討論衝突點彙整

---

## BRA 論文價值評估

### 初始評估

BRA 具備論文價值，理由：
1. 問題定義清晰：指出 AI 系統缺乏結構化行為控制機制
2. 理論創新：分層語言模型（NNL → RNL → Rule）、語義漂移控制
3. 工程實作價值：Rule/Ruleset/Boundary 執行架構、MUST/MUST NOT 區分

### 資料完備程度

已完備：理論架構、語義規範、CODE Rule Catalog 範例、論文大綱

待補充：
1. 相關工作回顧（Related Work）：需對比 AI 安全框架（Guardrails AI、LangKit 等）或 PBAC
2. 評估與實驗數據（Evaluation）：對比實驗、效能開銷
3. 實作細節：Rule Schema、Boundary Resolver 的演算法邏輯

### 論文定位建議

資料足以支持：
- 高品質技術報告（Technical Report）
- 概念驗證論文（Vision Paper）
- 若投稿頂級會議（ICSE、FSE）需補強實驗章節

---

## BCM 治理框架

### BCM 介紹

BCM（Behavior Control Model）為 BRA 的治理框架互補：

| 比較 | BRA | BCM |
|------|-----|-----|
| 角色 | 工程架構（工具、語言、靜態結構） | 治理框架（動態循環、風險評估、決策機制） |

### BCM 章節結構（15 章）

BCM 文件包含 15 個章節：
1. Introduction：AI 行為治理問題與 BCM 研究動機
2. The AI Behavior Governance Problem：界定治理問題
3. Position of BCM in the AI Engineering Framework：五層架構定位
4. Behavior Scope and Execution Context：Visible Scope / Artifact Scope / Behavior Scope
5. Ruleset Activation Model：Behavior Scope → Ruleset Activation → Rule Enforcement
6. Decision Risk Integration：Execution Behavior → Decision Analysis → Decision Risk → Governance Response
7. Governance Control Cycle：PDCA 循環（Ruleset Design → AI Execution → Risk Observation → Rule Adjustment）
8. Observability and Auditability：Behavior Logging、Rule Traceability、Risk Audit、Governance Evidence
9. Implementation Architecture：Rule Engine、Ruleset Manager、Risk Analyzer、Governance Controller
10. Implications for AI Engineering
11. Limitations
12. Conclusion
13. Relationship to BRA and Boundary：Control Pipeline（VSS → Boundary → BRA → BCM）
14. Related Work
15. Future Work

### BRA + BCM 結合的論文價值

結合後的優勢：
1. 完整控制鏈結（Control Pipeline）：`VSS → Boundary → BRA → BCM`
2. 動態性與閉環治理：PDCA 循環 + Decision Risk
3. 解決「黑盒執行」問題：Behavior Scope 結構化 AI 的能力範圍與權限邊界

待強化：
1. 風險量化模型：需補充評估公式或風險矩陣
2. 案例對比：具體情境（如 AI 嘗試修改核心 API 時 BCM 的處理）
3. 實證/可觀測性證明：模擬 Governance Audit Log

### 兩篇論文 vs 合併論文

- 方案 A（兩篇）：BRA 專攻 SE、BCM 專攻 AI Governance/Safety
- 方案 B（合併 — Gemini 推薦）：標題建議 _From Constraints to Governance: A Unified Behavior Control Model for AI-Assisted Engineering_

---

## 工程架構論文章節結構

### 標準敘事流程

工程架構論文遵循「發現痛點 → 提出模型 → 形式化定義 → 實作驗證」：

1. Introduction：Context → Problem Statement → Gap → Contributions
2. Background & Related Work
3. Motivation & Design Principles：Motivating Example + Core Principles
4. The Proposed Architecture（BRA 靜態結構）：Language System、Structural Representation、Formalization
5. Governance & Control Model（BCM 動態邏輯）：Behavior Scope & Activation、Decision Risk、PDCA Cycle
6. Implementation：System Overview、Toolchain、Integration
7. Evaluation / Case Study（關鍵章節）
8. Discussion & Limitations
9. Conclusion

### 論文貢獻（Gemini 建議）

四個維度：
1. 理論框架：分層行為語言體系、行為邊界模型
2. 工程架構：BRA（原子化 Rule + 動態 Ruleset 組合）、Ruleset 啟動與衝突解邏輯
3. 治理與安全：BCM PDCA 治理循環、Decision Risk 整合、可審計性
4. 實證與應用：CODE Rule Catalog

---

## CODE Rule Catalog

### 設計原則

五個不可協商原則：
1. Code-Level Only：規則僅適用於程式碼產出物
2. Scenario-Free：規則不編碼 add/change/fix/refactor 等意圖
3. Boundary-First and Fail-Stop：邊界缺失/衝突/無效時 MUST 立即停止
4. Composable and Reusable：每條規則原子化、可跨 Ruleset 重用
5. Execution Rules vs. Governance Guardrails：MUST（工程義務）vs MUST NOT（硬性禁令）

Execution Rules（MUST）vs Governance Guardrails（MUST NOT）：
- Execution Rules：定義工程義務，違反可修補
- Governance Guardrails：定義硬性禁令，違反構成 evidence-based governance trigger，MUST 執行停止、記錄、呈報
- **唯有違反 Governance Guardrails (MUST NOT) 產生硬性治理觸發**

### 規則分類（七大類別）

| 分類代碼 | 名稱 | 風險領域重心 |
|----------|------|-------------|
| TR | Traceability | 追蹤標識符與非孤兒程式碼 |
| BD | Boundary & Stop | 執行邊界與硬性停止條件 |
| AR | Artifact Isolation | 跨產出物交叉污染 |
| ST | Structural Change | 結構與行為完整性 |
| TI | Test Integrity | 測試產出物誤用 |
| CN | Constraint Neutrality | 過度推論與範圍擴張 |
| LG | Logging & Report | 執行透明度與證據完整性 |

BD 類別全部為治理護欄。治理相關風險領域至少須包含一條 MUST NOT。

### 命名規範

格式：`CODE-<CATEGORY>-<NN>`
- Rule ID 穩定，不因 Ruleset 組合而改變
- 未來領域使用獨立命名空間（如 `docSCR-DOC-*`、`vpSCR-VIEWPOINT-*`）

### CODE Rule Catalog 在論文中的呈現

- 少量規則（10-15 條）：放正文
- 大量規則（20+）：核心 5 條放正文，完整清單放附錄
- 學術化調整：術語統一、分類排序（先 BD 再 TR）、文字精煉

### Rule 完整列表的價值

1. 建立「可審計 AI」的分類學（Taxonomy）
2. 實現「防禦性編程」的 AI 版本（最小權限原則）
3. 量化治理完整性（Rule Statistics 統計表）
4. 解決「語義漂移」的實證證據

---

## 概念架構更新

### 更新後的演進鏈

```
NNL → Prompt
↓
RNL → Behavior Rule
↓
Rule → RNL + Governance Meta
```

### 資產管理與工程分類

- Rule Library：跨產業/企業/專案共享的儲存庫（打破孤島）
- Rule Category：依工程應用領域分類（CODE、DOC 等）

### 組裝與啟動機制

- Ruleset：企業依 SOP 自組、可跨分類整併
- Boundary / Behavior Scope：動態指定適用 Ruleset，支持一組或多組

### 更新後的優勢

1. 資產化（Assetization）：Rule 成為可管理、審計、共享的數位資產
2. 解耦（Decoupling）：Library vs Application、RNL vs Governance Meta
3. 可擴展性：透過 Rule Category 擴展至法務、醫療等領域

---

## Policy vs Constraint 的設計演進

### 早期設計

使用 "Constraint" 統稱所有規範語句，未區分 MUST 和 MUST NOT。

### 實作發現

| 維度 | MUST（Policy） | MUST NOT（Constraint） |
|------|----------------|------------------------|
| 觸發可觀測性 | 無法觀測 | 可以觀測 |
| Evidence 生成 | 不會留下 | 可以留下 |
| 對 AI 的意義 | 期望性行為 | 限制性行為 |
| 強制性等級 | 中等 | 高 |
| 驗證策略 | Output review（事後驗證） | Runtime detection（即時偵測） |

### Gemini 的強烈建議

採用方案 2：**區分 MUST (Policy) 與 MUST NOT (Constraint)，以 Rule 統稱。**

理由：
1. 論文敘事優勢：可驗證性、建立治理語義學創新點
2. 符合 Safety-Critical Systems 慣例（Liveness vs Safety）
3. 業界使用習慣：Policy = 指導方針、Constraint = 絕對禁止
4. 實作上的阻斷機制：Constraint → Hard Stop、Policy → Warning

### Gemini 的三層語義模型建議

Gemini 建議擴展為三層：
1. Constraint (MUST NOT)：硬性護欄
2. Policy (MUST)：期望義務
3. Guidance (SHOULD)：建議引導（Gemini 建議，但當前設計維持二層）

---

## DevOps 與 Runtime 延伸

### DevOps Stage

Rule Library 中的規則融入 CI/CD：
- 自動化準入規則（如 CODE-TR-05 在 Pipeline 中檢查）
- 動態 Ruleset 組合（依 Deployment 環境）

### Runtime Stage

實時行為攔截：
- Behavior Boundary Monitoring
- Risk Signal Feedback Loop（BCM 核心）

### 全生命週期映射

| 階段 | 角色 | 核心物件應用 |
|------|------|-------------|
| Develop | 指令定義 | NNL → Behavior Rules |
| DevOps | 合規檢查 | 自組 Ruleset 作為 CI/CD 過濾器 |
| Runtime | 行為防護 | Behavior Scope 動態綁定 Ruleset |

### Rule Library 的角色

Rule Library 作為「單一事實來源（Single Source of Truth）」：
- 同一 RNL 描述在不同階段有不同角色（提示 / 檢核項 / 攔截條件）

---

## 14 個待討論衝突點

### 衝突點概覽

| 編號 | 議題 | 處理優先級 |
|------|------|-----------|
| 2.1 | RNL 關鍵字範圍（SHOULD/SHOULD NOT） | 優先 |
| 2.2 | execution_view 概念 | 次要 |
| 2.3 | Risk Potential 三層風險模型 | 優先 |
| 2.4 | Attribute vs Imperative 分離 | 次要 |
| 2.5 | Risk Trigger 與 Required Behavior | 次要 |
| 2.6 | Rule Relations（互斥/依賴/分組） | 可選 |
| 2.7 | Validation 三層模型 | 可選 |
| 2.8 | Schema Metadata | 可選 |
| 2.9 | Wording 建議 | 可選 |
| 2.10 | Category vs Application Ruleset 邊界 | 優先 |
| 2.11 | Boundary 位置與定義 | 優先 |
| 2.12 | GES 定位 | 可選 |
| 2.13 | Ruleset 意識性 | 可選 |
| 2.14 | CNL 規範 | 次要 |

### RNL 關鍵字範圍（2.1）

早期設計包含 MUST / MUST NOT / SHOULD / SHOULD NOT，當前僅 MUST / MUST NOT。

Gemini 建議擴展為三層語義模型（Policy / Constraint / Guidance），但用戶當前設計維持二層。SHOULD 視為 NNL 範疇。

### execution_view（2.2）

RSG.md 明確定義：
- What it IS：解釋視角（perspective/lens）、interpretation emphasis hint
- What it IS NOT：責任分配、角色扮演、Human-in-the-loop、執行權限

Gemini 建議引入，採用 RSG.md 定義。

### Risk Potential 三層模型（2.3）

三層：Risk Potential（先驗）→ Risk Signal（執行中）→ Realized Risk（稽核）

Gemini 建議：
- Metadata 欄位統一使用 `risk_potential`
- 拒絕 RSG.md 在 `applicability_context` 放 `severity` 的做法
- 維持 devSCR 的嚴謹性

### Attribute vs Imperative 分離（2.4）

核心原則：attribute 形式存在的信息屬於 governance element，非 execution directive。

Gemini 建議嚴格執行，所有 Metadata 欄位標記為 Attribute，強調「Attribute 僅供治理機器讀取，不直接進入 AI 的指令上下文」。

### Risk Trigger 與 Required Behavior（2.5）

核心概念：Risk trigger ≠ Risk judgment，Risk trigger ⇒ Required behavior。

Gemini 建議將 Risk Trigger 視為 Boundary 的動態偵測條件。

### Rule Relations（2.6）

早期設計定義了互斥（R4 ⊥ R5）、依賴（R1 → R2）、分組（{R1,R2,R3} ∈ G）。

Gemini 認為增加複雜度，可暫不引入。

### Validation 三層模型（2.7）

三層：
1. Artifact Structure Validation（JSON Schema validator，不碰 CNL/語意）
2. CNL Conformance Validation（quality gate，可設定 strict/advisory 模式）
3. Authoring Quality Advisory（永遠不 fail，只產生 warning/info）

Gemini 認為屬於工具層，非 BRA 核心範圍。

### Schema Metadata（2.8）

做法 A（推薦）：移到註解
做法 B：保留 `meta:` 但工具可忽略

### Wording 建議（2.9）

| 原詞 | 建議 |
|------|------|
| Intent | `governance_purpose` / `policy_intent` |
| Roles | `responsibility_roles` / `governance_role` |
| Context | `applicability_context` / `governance_context` |
| Effect | `governance_impact` / `policy_implication` |

最小侵入建議：使用 `x-meta` 標註語義角色，不改欄位名稱。

### Category vs Application Ruleset 邊界（2.10）

三鐵律（與 Claude 對話一致）：
1. Ruleset 不得定義 Policy 或 Constraint
2. Ruleset 是「視角」不是「控制器」
3. Ruleset 必須能被多系統重用

正確分層：Language Layer → Behavior Category → Rule Layer → Asset Layer → Application Ruleset

三個關鍵原則：Composition Scope、Category Isolation、Atomic Rule Integrity

### Boundary 位置（2.11）

Gemini 建議「雙層宣告」：
1. Rule 內部：靜態 Boundary 宣告（適用的 Artifact Type）
2. Layer 4 (Boundary)：動態 Scope Binding（具體路徑/專案）

### GES 定位（2.12）

Governance Evidence Specification (GES) 由 RuleSpecification.md 提及，當前 concept.md 未包含。Gemini 建議作為未來工作方向。

### Ruleset 意識性（2.13）

Gemini 建議 Rule 必須對 Ruleset「無意識」，維持單向引用（Ruleset → Rule），反向查詢由 Rule Library 工具層透過索引達成。

### CNL 規範（2.14）

需決定是否在論文中定義 RNL 的語法規範（關鍵字、句式模板、禁止結構），以及 RNL 是完全機器可解析還是人類可讀優先。

---

## Gemini 彙整建議

### 建議總覽

Gemini 對 14 個衝突點提出七個彙整建議：

1. **三層語義模型**：Rule 下分 Constraint / Policy / Guidance（2.1）
2. **嚴格 Attribute/Imperative 分離**：Metadata 欄位標記為 Attribute（2.4）
3. **三層風險模型命名 risk_potential**：拒絕 RSG.md 做法（2.3）
4. **Risk Trigger 作為 Boundary 擴展**（2.5）
5. **引入 execution_view 採用 RSG.md 定義**（2.2）
6. **維持三鐵律 + Boundary 雙層宣告**（2.10 & 2.11）
7. **Ruleset 單向引用**（2.13）

### 論文修改清單

1. 術語定調：`Rule = ⟨RNL, Metadata, Boundary_Declaration⟩`
2. 層級劃分：Guidance (SHOULD) 屬於 NNL 到 RNL 過渡區，Rule 核心僅 Policy + Constraint
3. 新增圖表："Risk Lifecycle in BRA"（Risk Potential → Risk Trigger → Realized Risk）

---

## Outline 評估

### 整體評價

Gemini 評定為「優等」Outline：
- 結構嚴謹、層次分明
- 核心創新：語義脫鉤、可組合性
- 實踐導向

### 調整建議

1. 強化「行為對稱性」理論支持：MUST vs MUST NOT 的非對稱性寫入 Rule Layer
2. 深化 Boundary 動態性：增加 Dynamic vs. Static Boundary Binding 小節
3. 增加「語義漂移」量化對比：Group A（純 Prompt）vs Group B（RNL + Ruleset）
4. 術語一致性：統一使用 RNL，註明基於 CNL 原則設計

### 各章節補充建議

| 章節 | 建議補充 |
|------|---------|
| Section 3 (NNL) | 人類意圖模糊性如何透過 NNL 責任對齊被過濾 |
| Section 5 (Library) | Category Ruleset = 領域標準化，Application Ruleset = 專案特化 |
| Section 7 (Lifecycle) | Rule 廢棄與版本更新機制 |
| Section 10 (Implications) | 擴展至 DevOps CI/CD Pipeline |

### 論文金句建議

> "Unlike traditional prompt engineering that relies on implicit inference, Behavior Rule Architecture (BRA) transforms AI governance from 'black-box prompting' to 'transparent, observable, and composable engineering assets'."

---

## RulesetSpecification 補充素材

### Ruleset Mental Model（治理信封）

A governance envelope that gives multiple Rules a shared identity, without changing what any Rule means.

### 最小 Ruleset 概念

最小 Ruleset 已具備治理價值：
- 提供治理控制柄（governance handle）
- 穩定的分組身份
- 工具整合的參考邊界

### Ruleset Validation Model

檢查：結構正確性、必要欄位存在性、有效 Rule 引用、Governance metadata 形狀

不檢查：約束正確性、政策意圖、風險適用性、執行可行性

### Ruleset Versioning and Lifecycle

- 獨立版本化（更新 Rule 不自動變更 Ruleset 版本）
- 生命週期狀態僅為描述性，不影響執行行為
- Ruleset 作為穩定的治理快照

---

## 編輯檢查

- [x] 依原始對話時間排序
- [x] 保留所有有意義內容
- [x] 只去除噪音，非刪除語意
- [x] 保留推論過程與被推翻的推論
- [x] 將對話痕跡改寫為可閱讀的筆記結構
- [x] 避免重複敘述同一內容
