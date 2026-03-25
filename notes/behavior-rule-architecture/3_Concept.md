# Concept

## 1. 整體概念鏈（Refined Architecture Chain）

```
================================================================================
                      Behavior Rule Architecture (BRA)
             AI Execution Behavior Normative Engineering Framework
================================================================================

┌──────────────────────────────────────────────────────────────────────────────┐
│                       1. Rule Language Layer                                 │
│                (Input Abstraction & Normative Constraint)                    │
├──────────────────────────────────────────────────────────────────────────────┤
│   NNL  ──────────────────────────────►  RNL                                  │
│   Normative Natural Language             Rule Normative Language             │
│   • Prompt Optimization                  • Behavior Rule Expression          │
│   • Human-readable intent                • MUST / MUST NOT normative rules   │
│   • Non-enforceable                      • Enforceable behavior normative    │
└──────────────────────────────────────────────────────────────────────────────┘
                                      ↓
                                      │
┌──────────────────────────────────────────────────────────────────────────────┐
│                       2. Rule Architecture Layer                             │
│                     (Structured Rule Definition)                             │
├──────────────────────────────────────────────────────────────────────────────┤
|   Behavior Category ───────────────────► Rule                                │
│   • Behavior Normative Classification   • Structured Governance Artifact    │
│   • 先定義行為限制類別，再衍生對應規則      • RNL + Governance Metadata         │
└──────────────────────────────────────────────────────────────────────────────┘
                                      ↓
                                      │
┌──────────────────────────────────────────────────────────────────────────────┐
│                      3. Governance Asset Layer                               │
│                 (Reusable Shared Rule Repository)                            │
├──────────────────────────────────────────────────────────────────────────────┤
│                         Rule Library                                         │
│          (Cross-industry / Cross-enterprise / Cross-project Repository)      │
│                                                                              │
│  Ruleset  =  Pure Rule Collection                                            │
│                                                                              │
│  • Category Ruleset : 基於相同 Behavior Normative 的所有 Rule 集合            │
│    （例如：所有 TR - Traceability 相關的 Rule）                               │
│                                                                              │
│  • Application Ruleset : 基於任意原則設計的規則集合體                          │
│    （可為 Workflow-based、Domain-based、Risk-based、Compliance-based、        │
│     或任何產業/企業自訂原則，原則種類無上限）                                   │
└──────────────────────────────────────────────────────────────────────────────┘
                                      ↓
                   External Integration / Application Layer (非 BRA 核心範圍)
                                      │
┌──────────────────────────────────────────────────────────────────────────────┐
│          4. External Execution & Control Layer（架構生態系統）                │
├──────────────────────────────────────────────────────────────────────────────┤
│  ⚠️ Boundary Architecture                                                    │
│  • Execution Boundary Binding（執行範疇綁定）                                │
│  • Runtime Context Binding（運行環境綁定）                                   │
│  • Behavior Scope Binding（行為範圍綁定）                                    │
│                                                                              │
│                     ↓                                                        │
│                                                                              │
│  ⚠️ BCM (Behavior Control Model)                                             │
│  • Runtime Enforcement（運行階段強制執行）                                   │
│  • Risk Signal Detection（風險訊號偵測）                                     │
│  • Evidence Generation（執行證據生成）                                       │
│                                                                              │
│                     ↓                                                        │
│                                                                              │
│  ❌ External Tools                                                           │
│  • Validation（驗證工具）                                                    │
│  • Schema Management（Schema 管理）                                          │
│  • RNL Tools（RNL 工具）                                                     │
│  • GES（Evidence 結構定義）                                                  │
└──────────────────────────────────────────────────────────────────────────────┘
```

---

## 2. 數學表示法
> 描述 Rule, Rule Library, Ruleset, Category Ruleset, Application Ruleset 之間的數量關聯

### 2.1 基數關係（Cardinality Relationships）

#### 2.1.1 Rule ↔ Category Ruleset（一對多）
```
Category_Ruleset : Rule  =  1 : N

∀ rule ∈ Library:
  ∃! category ∈ Behavior_Category:
    rule ∈ Category_Ruleset(category)
```
即：每條 Rule 只能屬於一個 Behavior Category；反之，一個 Category Ruleset 可包含多條 Rule。

#### 2.1.2 Rule ↔ Application Ruleset（多對多）
```
Rule : Application_Ruleset  =  N : N

∀ rule ∈ Library:
  ∃ {P₁, P₂, ..., Pₖ} ⊆ Principle:
    rule ∈ Application_Ruleset(Pᵢ), ∀ᵢ

∀ Application_Ruleset:
  ∃ {rule₁, rule₂, ..., ruleₘ} ⊆ Library:
    ruleⱼ ∈ Application_Ruleset
```
即：一條 Rule 可被組合進多個 Application Ruleset；一個 Application Ruleset 可組合多條 Rule。

#### 2.1.3 Ruleset 基數約束
```
∀ Ruleset ⊆ Library:
  |Ruleset| ≥ 1
```
即：一個 Ruleset 至少有一條 Rule，最多無上限。

---

### 2.2 組合關係（Composition Relationships）

#### 2.2.1 Rule Library 與 Category Ruleset
```
Library = ⋃_{C ∈ Behavior_Category} Category_Ruleset(C)
```
即：所有 Category Ruleset 的聯集構成完整的 Rule Library。

#### 2.2.2 Category Ruleset 互斥性
```
∀ C₁ ≠ C₂ ∈ Behavior_Category:
  Category_Ruleset(C₁) ∩ Category_Ruleset(C₂) = ∅
```
即：不同 Behavior Category 的 Category Ruleset 互不重疊（互斥分區）。

#### 2.2.3 Application Ruleset 與 Category Ruleset 交集
```
∀ C ∈ Behavior_Category, ∀ P ∈ Principle:
  Category_Ruleset(C) ∩ Application_Ruleset(P) = ∅  或  ≠ ∅
```
即：同一條 Rule 可同時屬於某個 Category Ruleset 與 Application Ruleset，但此交集不具強制約束，取決於具體組合。

---

### 2.3 實體關係圖（Entity-Relationship Diagram）

```
┌─────────────────────────────────────────────────────────────────────┐
│                           Rule Library                               │
│                    Library = ⋃ Category_Ruleset(C)                  │
└────────────────────────────┬────────────────────────────────────────┘
                             │
                             │ 包含（分區）
                             │
              ┌──────────────┴──────────────┐
              │                             │
    ┌─────────▼─────────┐         ┌────────▼────────┐
    │ Category Ruleset  │         │ Application     │
    │                   │         │ Ruleset         │
    │ 基於 Behavior     │         │ 基於任意原則    │
    │ Constraint 分類   │         │ (Workflow,      │
    │                   │         │  Domain, etc.)  │
    └─────────┬─────────┘         └────────┬────────┘
              │                             │
              │ 1:N                         │ N:N
              │ (一個 Category              │ (一個 Application
              │  Ruleset 包含多條 Rule)     │  Ruleset 包含多條 Rule,
              │                             │  一條 Rule 可屬於多個
              │                             │  Application Ruleset)
              │                             │
    ┌─────────▼─────────────────────────────▼────────┐
    │                   Rule                         │
    │           結構化治理單元                        │
    │           Rule = ⟨RNL, Metadata⟩              │
    └────────────────────────────────────────────────┘
```

---

### 2.4 映射關係總結

| 關係                                   | 類型   | 說明                                         |
| -------------------------------------- | ------ | -------------------------------------------- |
| Rule → Category Ruleset                | 1:N    | 每條 Rule 只屬於一個 Behavior Category       |
| Category Ruleset → Rule                | 1:N    | 每個 Category Ruleset 可含多條 Rule          |
| Rule → Application Ruleset             | N:N    | 一條 Rule 可被組合進多個 Application Ruleset |
| Application Ruleset → Rule             | N:N    | 一個 Application Ruleset 可含多條 Rule       |
| Library ↔ Category Ruleset             | 分解   | Library = 所有 Category Ruleset 的聯集       |
| Category Ruleset ∩ Category Ruleset    | ∅      | 不同 Category 的 Ruleset 互斥                |
| Category Ruleset ∩ Application Ruleset | ∅ 或 ∃ | 可能有交集，但不必然                         |

---

## 3. 核心概念定義（Formalized Definitions）

### 3.1 NNL（Normative Natural Language）
**定義**
NNL 為人類可讀的自然語言，用於表達任務意圖與操作描述。

**特性**
- 非強制性（non-enforceable）
- 高語義彈性
- 易產生 semantic drift

**角色**
→ 僅作為 prompt input，不參與行為控制

**對比現行方法**
- Prompt Engineering：完全依賴 NNL，缺乏結構化約束
- Custom Instructions：NNL 片段化，無法確保執行語義
- Rules Files（AGENTS.md、Cursor Rules）：仍以 NNL 表達，非強制性

---

### 3.2 RNL（Rule Normative Language）
**定義**
RNL 為一種規範語言，用於表達可執行的行為規則，透過 MUST / MUST NOT 等語句定義 AI 行為政策或約束。

**形式**
- MUST：強制性政策（Policy）
- MUST NOT：禁止性約束（Constraint）

**為何區分 MUST 與 MUST NOT**
早期設計僅以 Constraint 統一表示所有規範語句，未區分 MUST 與 MUST NOT。
在實際實作與觀測中發現兩者具有本質差異：

| 特性     | MUST（Policy）      | MUST NOT（Constraint） |
| -------- | ------------------- | ---------------------- |
| 觸發觀測 | 無法觀測（期望式）  | 可觀測（違反即可偵測） |
| Evidence | 不產生直接 Evidence | 觸發時可留下 Evidence  |
| AI 語義  | 期望（Expectation） | 限制（Restriction）    |
| 強制性   | 中等                | 高                     |

因此，BRA 將 Rule 區分為 Policy（MUST）與 Constraint（MUST NOT）兩種子類型，
以反映其在可觀測性、可審計性與強制力上的根本差異。

**角色**
→ Behavior Rule 的語義層，確保行為可執行性

**對比現行方法**
- Policy-as-Code：部分接近 RNL 概念，但多為事後檢查，非 pre-execution constraint
- LLM Guardrails：多為 output filtering，未深入行為生成前的約束

---

### 3.3 Decision Behavior Normative Category（決策行為規範類別）
**定義**
Decision Behavior Normative Category 為「對 AI 決策行為規範的分類機制」，而非「對 AI 行為本身的分類」。先界定決策行為規範類別，再從該類別衍生對應規則。

**關鍵特性**
- 以「對決策行為的規範類型」為分類核心（非 Domain Application，亦非單純的行為類型）
- 一個 Decision Behavior Normative Category 對應一個 Category Ruleset
- Category Ruleset 與 Rule 為 N:1 關係（每條 Rule 只屬於一個 Category）
- 每個 Category 至少包含一條 Constraint（MUST NOT），確保該類別具備可觀測的行為邊界

**範例（決策行為規範類別）**
- AR（Artifact Isolation）：產出物隔離決策規範
  - 例如：必須確保產出物獨立性、禁止跨邊界污染
- BD（Boundary & Stop）：邊界與停止決策規範
  - 例如：必須在邊界內執行、觸發停止條件時必須終止
- CN（Constraint Neutrality）：約束中立決策規範
  - 例如：約束必須無歧義、禁止隱含假設
- TR（Traceability）：可追溯性決策規範
  - 例如：必須保留執行軌跡、禁止破壞審計鏈
- LG（Logging & Report）：日誌與報告決策規範
  - 例如：必須記錄關鍵操作、禁止遺漏必要報告
- ST（Structural Change）：結構變更決策規範
  - 例如：結構變更必須經過審核、禁止破壞性重構
- TI（Test Integrity）：測試完整性決策規範
  - 例如：必須維持測試獨立性、禁止測試間依賴

**與 Domain Category 的差異**
- **Decision Behavior Normative Category**：先定義「對決策行為的規範類型」，再設定規則（BRA 核心設計）
- **Domain Category**：先設計 Rule，再看 Rule 可應用在哪個 Domain（傳統方法）

**對比現行方法**
- 現行方法缺乏明確的決策行為規範分類，Rule 片段散落在不同配置中
- BRA 透過 Decision Behavior Normative Category 建立可重用、可治理的行為規範分類架構

---

### 3.4 Rule（Structured Rule Artifact）
**定義**
Rule 為結構化治理單元，由 RNL + Governance Metadata + Boundary Declaration 組成，為 BRA 的最小可管理單元。

**形式化**
```
Rule = ⟨RNL, Metadata, Boundary_Declaration⟩
```

**分類**
- Policy: MUST（強制執行的行為政策）
- Constraint: MUST NOT（禁止執行的行為約束）

**分類設計理由**
早期版本以 Constraint 統稱所有規則，未區分語義方向。實作驗證後發現：
- **Policy（MUST）**：定義 AI 應達成的期望行為。觸發為隱性——AI 若遵循則行為符合期望，但「已遵循」本身難以直接觀測或產生 Evidence，強制性中等。
- **Constraint（MUST NOT）**：定義 AI 禁止執行的行為。觸發為顯性——AI 若違反即可被偵測，能產生明確的 Evidence（如違規日誌、行為偏差記錄），強制性高。

此區分使得治理系統可針對兩類規則採用不同的驗證策略：
- Policy 適合透過 output review 與 compliance check 驗證
- Constraint 適合透過 runtime detection 與 violation evidence 驗證

**Metadata 結構**
- version：版本號（支援演進）
- status：生命週期狀態（draft / active / deprecated）
- owner：負責人
- intent：規則意圖說明（WHY）
- responsibilities：適用責任範圍（WHO）
- **execution_view**：（選填）AI 解讀此 Rule 的視角（如：security、qa、architecture）
  - 目的：減少規則解釋歧義
  - 注意：這是解讀濾鏡，不是執行角色或權限控制
  - 與 Behavior Category 互補而不重疊
- **risk_potential**：先驗風險評估（BRA 範疇）
  - impact_score：潛在影響評分（1-5）
  - rationale：影響理由
  - 注意：BRA 僅負責先驗評估
  - ⚠️ 實際風險訊號（Risk Signal）與實現風險（Realized Risk）由 BCM 和治理層負責（三層風險模型）
- applicability_context：治理相關的適用情境（WHEN）
  - ⚠️ 不應被解釋為觸發條件或執行邏輯（BCM 範疇）
- governance_impact：預期的治理層級結果（WHAT HAPPENS）
- impact：影響範圍

**Boundary Declaration（靜態邊界宣告）**
Rule 內部可包含靜態的 boundary 宣告，用於標註規則的語義適用性：
```yaml
boundary:
  artifact_type: source_code    # 適用的產出物類型
  source_ref: STS-JWT-001      # 參考的規格或需求（選填）
```
- **目的**：提供 Rule 的自包含性，便於 Rule Library 分類與查詢
- **⚠️ 注意**：這是 BRA 範疇的靜態宣告，實際的執行範疇綁定由 **Boundary Architecture** 的 Execution Boundary 控制
- **設計理由**：雙層設計兼顧 Rule 的自包含性與執行的靈活性
  - ✅ Rule-level Boundary（BRA）：靜態語義適用性宣告
  - ⚠️ Execution Boundary（Boundary Architecture）：動態執行範疇綁定
**特性**
- 可審計（auditability）：執行過程可追溯
- 可追蹤（traceability）：與執行證據關聯
- 可版本化（version control）：支援規則演進
- 獨立性（atomicity）：不依賴其他 Rule

**對比現行方法**
- Prompt Engineering：無結構化 metadata，難以治理
- Custom Instructions：片段化配置，無統一管理
- Rules Files：開始有結構化趨勢，但未達治理資產層級

---

### 3.5 Category Ruleset（Behavior-Based Rule Collection）
**定義**
Category Ruleset 為基於相同 Behavior Normative 下的所有 Rule 集合。

**數學表示**
```
Category_Ruleset : Behavior_Category → 𝒫(Rule)

Category_Ruleset(C) = {rule ∈ Library | constraint_type(rule) = C}

基數關係：Category_Ruleset : Rule = 1:N
```

**特性**
- 不同 Behavior Category 的 Category Ruleset 互斥（不重疊）
- 所有 Category Ruleset 的聯集構成完整的 Rule Library
- **Category 僅負責分類，不涉及規則組合**（組合由 Application Ruleset 負責）

**範例**
```
C_tr = "Traceability"
Category_Ruleset(C_tr) = {
  rule_must_preserve_execution_trail,
  rule_must_not_break_audit_chain,
  rule_must_include_trace_metadata,
  ...
}
```

---

### 3.6 Application Ruleset（Principle-Based Rule Collection）
**定義**
Application Ruleset 為基於任意應用原則所設計的規則集合體，可為 Workflow-based、Domain-based、Risk-based、Compliance-based 或任何產業/企業自訂原則。

**數學表示**
```
Application_Ruleset : Principle → 𝒫(Rule)

Application_Ruleset(P) = {rule ∈ Library | satisfies(rule, P)}

基數關係：Rule : Application_Ruleset = N:N
```

**原則類型（無上限）**
- Workflow-based：依企業作業流程組合（開發、測試、部署）
- Domain-based：依應用領域整合（安全、測試、文件）
- Risk-based：依風險等級篩選
- Compliance-based：依合規要求組合
- Custom：企業/產業自訂原則

**特性**
- 一條 Rule 可被組合進多個 Application Ruleset
- 一個 Application Ruleset 可組合多條 Rule
- 與 Category Ruleset 可能有交集，但不必然

**範例**
```
P_dev = "Development Workflow"
Application_Ruleset(P_dev) = {
  rule_version_control,
  rule_code_review,
  rule_must_use_linter,    // 來自 TR Category
  rule_must_include_tests,  // 來自 VR Category
  ...
}
```

**對比現行方法**
- 現行方法缺乏動態組合能力，每次執行只能套用單一配置
- BRA 透過 Application Ruleset 支援基於不同原則的靈活組合

---

### 3.6.1 Category vs Application Ruleset：三層鐵律

為確保 BRA 架構的清晰性與靈活性，定義以下三條鐵律：

**🔒 鐵律 1：Category MUST NOT Define Rule Composition**
- Category 僅負責基於 Behavior Normative 的分類
- Category 不應定義或建議任何規則組合
- 規則組合權完全屬於 Application Ruleset

**🔒 鐵律 2：Rule Composition MUST Occur Only in Application Ruleset**
- 規則的自由組合僅發生在 Application Ruleset 層級
- Application Ruleset 可基於任意原則（Workflow、Domain、Risk、Compliance 等）組合規則
- 一條 Rule 可被組合進多個 Application Ruleset

**🔒 鐵律 3：Rules MUST Remain Atomic and Independent**
- 每條 Rule 必須保持原子性與獨立性
- Rule 不應知道自己屬於哪些 Ruleset（單向引用：Ruleset → Rule）
- Rule 之間不應有互斥、依賴或分組關係（這類關係如需要，應在 Application Ruleset 層級定義）

**關鍵結論**
```
Categories describe Behavior Normatives（分類行為限制）
Rules enforce policies and constraints（執行政策與約束）
Applications decide composition（決定規則組合）
```

**職責邊界**
| 層級                | 職責                 | 不得涉及            |
| ------------------- | -------------------- | ------------------- |
| Behavior Category   | 行為限制分類         | 規則組合、應用邏輯  |
| Rule                | 定義單一政策或約束   | 組合關係、其他 Rule |
| Application Ruleset | 基於任意原則組合規則 | 定義新的政策或約束  |

---

### 3.7 Rule Library（Governance Asset Repository）
**定義**
Rule Library 為所有 Rule 的集中式儲存與管理系統，為可累積的治理資產。

**數學表示**
```
Library = ⋃_{C ∈ Behavior_Category} Category_Ruleset(C)
```

**特性**
- 所有 Rule 集合體
- 可跨：產業 / 企業 / 專案
- 長期累積治理資產
- 支援版本管理與演進

**演進路徑**
- Rule Pool：單一專案內的 Rule 集合
- Repository：組織內共享的 Rule 庫
- Marketplace：跨組織的 Rule 交易與共享平台

**對比現行方法**
- Prompt Engineering：無 shared library 概念
- Custom Instructions：片段化配置，難以跨專案共享
- Rules Files：開始有 repository-level 共享，但缺乏治理架構

---

## 4. 整體層級結構（Architecture Layering）

### 4.1 BRA 核心三層架構

**Layer 1 — Rule Language Layer（規則語言層）**

**核心功能**
- 輸入抽象與規範約束轉換
- NNL → RNL 語義收斂

**組成元素**
- NNL（Normative Natural Language）
  - Prompt Optimization
  - Human-readable intent
  - Non-enforceable
- RNL（Rule Normative Language）
  - Behavior Rule Expression
  - MUST / MUST NOT normative rules
  - Enforceable Behavior Normatives

**對比現行方法**
- 現行方法：NNL 直接作為行為控制，缺乏語義分離
- BRA：明確區分 Intent（NNL）與 Enforcement（RNL）

---

**Layer 2 — Rule Architecture Layer（規則架構層）**

**核心功能**
- 結構化規則定義
- Behavior Category → Rule 的衍生過程

**組成元素**
- Behavior Category
  - 對 AI 行為限制的分類
  - 先定義行為限制類別，再衍生規則
- Rule
  - Structured Governance Artifact
  - Rule = ⟨RNL, Metadata⟩
  - Policy (MUST) / Constraint (MUST NOT)

**對比現行方法**
- 現行方法：Rule 片段散落在 prompt、instructions、files 中
- BRA：統一結構化定義，具備治理能力

---

**Layer 3 — Governance Asset Layer（治理資產層）**

**核心功能**
- 可重用、可共享的規則儲存庫
- 規則分類與組合機制

**組成元素**
- Rule Library
  - Cross-industry / Cross-enterprise / Cross-project Repository
  - 治理資產累積與演進
- Category Ruleset
  - 基於相同 Behavior Normative 的 Rule 集合
  - Category_Ruleset : Rule = 1:N
- Application Ruleset
  - 基於任意原則的 Rule 組合
  - Rule : Application_Ruleset = N:N
  - Workflow-based / Domain-based / Risk-based / Compliance-based 等

**對比現行方法**
- 現行方法：缺乏 shared library，每次從頭配置
- BRA：建立可累積、可共享的治理資產

---

### 4.2 外部整合層（非 BRA 核心範圍）

**Layer 4 — External Execution & Control Layer**

**Boundary（雙層邊界設計）**

BRA 採用雙層 Boundary 設計，分離靜態宣告與動態綁定：

**1️⃣ Rule-level Boundary（靜態宣告）**
- 位於 Rule 內部的 `boundary` 欄位
- 定義語義適用性（semantic applicability）
- 目的：Rule 自包含性，便於 Library 分類與查詢
- 範例：
  ```yaml
  boundary:
    artifact_type: source_code
    source_ref: STS-JWT-001
  ```

**2️⃣ Execution Boundary（動態綁定）**
- 位於 Layer 4 的外部 Boundary 控制器
- 定義���行範疇（runtime scope）
- 目的：執行靈活性，支援不同場景的規則應用
- 功能：
  - 指定本次執行應適用的 Ruleset（可單組或多組）
  - Runtime Context Binding
  - Behavior Scope 約束

**關鍵定義**
```
Rule boundary defines semantic applicability（語義適用性）
Execution boundary defines runtime binding（執行範疇綁定）
```

**設計理由**
- 兼顧 Rule 的自包含性與執行的靈活性
- Rule 可在不同專案、不同場景重用
- 靜態宣告支援治理，動態綁定支援執行

**Execution（受控執行）**
- 受選取的 Ruleset 與 Behavior Scope 共同約束
- Constrained AI Action

**Evidence（執行證據）**
- Execution Log
- Rule-evaluation Report
- Trace ID
- Change Summary

**與 BRA 的關係**
- BRA 定義規則結構與治理
- Boundary / Execution / Evidence 為外部消費者（非 BRA 核心）

---

### 4.3 層級關係總覽

```
┌─────────────────────────────────────────────────────────┐
│  BRA Core Architecture（三層）                           │
├─────────────────────────────────────────────────────────┤
│  Layer 1: Rule Language Layer                          │
│  NNL → RNL                                              │
├─────────────────────────────────────────────────────────┤
│  Layer 2: Rule Architecture Layer                      │
│  Behavior Category → Rule                               │
├─────────────────────────────────────────────────────────┤
│  Layer 3: Governance Asset Layer                       │
│  Rule Library + Category Ruleset + Application Ruleset │
└─────────────────────────────────────────────────────────┘
                          ↓
┌─────────────────────────────────────────────────────────┐
│  External Integration（非 BRA 核心）                     │
├─────────────────────────────────────────────────────────┤
│  Boundary → Execution → Evidence                        │
└─────────────────────────────────────────────────────────┘
```

---

## 5. 關鍵創新點

### 5.1 語義分離（Semantic Separation）

**核心創新**
```
NNL ≠ RNL
Intent Optimization ≠ Behavior Enforcement
```

**對比現行方法**
- **Prompt Engineering**
  - 問題：NNL 同時承載 intent 與 constraint，易產生語義漂移
  - BRA：分離 intent（NNL）與 enforcement（RNL）
- **Custom Instructions**
  - 問題：NNL 片段化，無法確保執行語義
  - BRA：RNL 確保可執行的行為約束
- **Rules Files**（AGENTS.md、Cursor Rules）
  - 問題：仍以 NNL 表達，非強制性
  - BRA：RNL 具備 MUST / MUST NOT 規範語義

---

### 5.2 行為控制鏈條完整化（Complete Behavior Control Chain）

**核心創新**
```
從：prompt → pre-execution（斷裂）

轉為：prompt → constraint → rule → composition
      → scope → execution → evidence（完整鏈條）
```

**對比現行方法**
- **Prompt Engineering**
  - 缺乏結構化 constraint 與 composition
  - 無法形成完整的控制鏈
- **Custom Instructions**
  - 有片段化 constraint，但無 composition 機制
  - 缺乏 evidence generation
- **Policy-as-Code / LLM Guardrails**
  - 多為事後檢查（post-execution）
  - BRA 建立 pre-execution → runtime → post-execution 的完整鏈條

---

### 5.3 Governance 資產化（Governance as Asset）

**核心創新**
```
Rule 不再是：prompt 片段（不可管理）

而是：結構化治理資產（可管理、可累積、可共享）
```

**對比現行方法**
- **Prompt Engineering**
  - Rule 隱藏在 prompt 中，無法獨立管理
- **Custom Instructions**
  - Rule 片段化，難以跨專案共享
- **Rules Files**
  - 開始有 repository-level 共享，但缺乏治理架構
- **BRA 創新**
  - Rule Library：可累積的治理資產
  - Metadata：版本、負責人、風險等級、嚴重性
  - 演進路徑：Rule Pool → Repository → Marketplace

---

### 5.4 動態組合能力（Dynamic Composition Capability）

**核心創新**
```
Ruleset 支援：
- Category Ruleset（基於 Behavior Normative）
- Application Ruleset（基於任意原則）
```

**數學表示**
```
Rule ↔ Category Ruleset : 1:N（每條 Rule 只屬於一個 Category）
Rule ↔ Application Ruleset : N:N（多對多組合）
Library = ⋃ Category_Ruleset(C)
```

**對比現行方法**
- **Prompt Engineering**
  - 無 composition 機制，每次從頭撰寫
- **Custom Instructions**
  - 有固定配置，但無動態組合能力
- **Rules Files**
  - 有 repository-level rules，但缺乏基於原則的組合機制
- **BRA 創新**
  - Category Ruleset：基於 Behavior Normative 的規則分類
  - Application Ruleset：基於 Workflow、Domain、Risk、Compliance 等原則的靈活組合
  - 支援單次執行綁定多個 Application Ruleset

---

### 5.5 Behavior-Based 分類設計（vs. Domain-Based）

**核心創新**
```
Behavior Category：先定義對 AI 行為的限制類別，再衍生對應規則
Domain Category：先設計 Rule，再看 Rule 可應用在哪個 Domain
```

**對比現行方法**
- **現行方法**
  - 缺乏明確的決策行為規範分類機制
  - Rule 片段散落在不同配置中
- **BRA 創新**
  - 以決策行為規範類型為核心（AR、BD、CN、TR、LG、ST、TI 等）
  - 確保 Rule 與 AI 決策行為規範點精確對應
  - 建立可重用、可治理的決策行為規範分類架構

---

## 6. Memo

1. 新的設計 Category 不是 Domain Category，是 Behavior Category。兩者最大差別是 Behavior Category 先界定「對 AI 行為的限制類別」，再從這個行為限制類別設定規則；而 Domain Category 則是先設計 Rule，再看這個 Rule 可以應用在那個 Domain，且 Domain 和 Behavior Normative 可能有關，可能無關。
2. Ruleset 是單純指規則集合體
3. Application Ruleset 就是基於 Ruleset 原則基於某種原則設計的，可能包含Workflow-based, Domain-based，依那種原則並非只有這兩種，可能有無限種，完全取決於產業/企業的需求
4. Category Ruleset 則是基於相同 Behavior Normative 下的所有 Rule
5. 一個 Ruleset 至少有一條 Rule，最多沒有限制。
6. 一條 Rule 只可以屬於一個 Behavior Category，也就是說 Rule 和 Category Ruleset 是一對多。
7. 一條 Rule 可以被組合進多個 Application Ruleset，一個 Application Ruleset 可以組合多條 Rule，也就是說 Rule 和 Application Ruleset 是多對多。
8. 所有的 Category Ruleset 組合起來就是 Rule Library。

---

## 7. 架構生態系統

BRA 並非孤立存在，而是與多個外部架構共同構成一個完整的 AI 行為治理生態系統。

### 7.1 架構責任分離

**核心設計哲學**
```
BRA 應該只負責「定義行為約束」，
而不是「執行、判斷、治理或驗證」。
```

**架構分工**
```
✅ BRA：定義規則（開發階段）
⚠️ Boundary Architecture：綁定範疇（應用階段）
⚠️ BCM：執行規則（運行階段）
❌ External Tools：驗證與管理（工具階段）
```

### 7.2 Boundary Architecture

**核心責任**
```
✅ Execution Boundary Binding（執行範疇綁定）
✅ Runtime Context Binding（運行環境綁定）
✅ Behavior Scope Binding（行為範圍綁定）
✅ 動態範疇管理
```

**與 BRA 的關係**
- BRA 提供 Rule-level Boundary（靜態宣告）
- Boundary Architecture 提供 Execution Boundary（動態綁定）
- 兩者透過雙層設計分離關注點

### 7.3 BCM（Behavior Control Model）

**核心責任**
```
✅ Runtime Enforcement（運行階段強制執行）
✅ Risk Signal Detection（風險訊號偵測）
✅ Evidence Generation（執行證據生成）
✅ Constraint（MUST NOT）的即時執行
✅ Policy（MUST）的事後驗證
```

**三層風險模型**
```
Risk Potential (BRA) → Risk Signal (BCM) → Realized Risk (Governance)
先驗評估           → 執行偵測          → 實現風險
```

**Policy vs Constraint 的執行差異**
| 類型                  | 執行方式             | Evidence              | BCM 責任                            |
| --------------------- | -------------------- | --------------------- | ----------------------------------- |
| Policy (MUST)         | 期望執行，事後驗證   | ❌ 不產生直接 Evidence | ✅ 事後驗證（post-hoc verification） |
| Constraint (MUST NOT) | 即時執行，違規即偵測 | ✅ 產生違規 Evidence   | ✅ Runtime Enforcement               |

### 7.4 External Tools

**核心責任**
```
✅ Validation（驗證工具）
✅ Schema Management（Schema 管理）
✅ RNL Tools（RNL 工具）
✅ Evidence Structure Definition - GES（Evidence 結構定義）
```

**Validation 三層模型**
1. Artifact Structure Validation（結構驗證）
2. RNL Conformance Validation（RNL 規範驗證）
3. Authoring Quality Advisory（撰寫品質建議）

**與 BRA 的關係**
- 這些工具輔助 BRA 的開發與管理
- 但不屬於 BRA 核心架構
- 可獨立演進與替換

---

## 8. 核心設計原則

### 8.1 核心金句

**架構層級**
> **If it is injected, it is execution. If it is an attribute, it is governance.**

> **Governance does not depend on what is described, but on whether it is declarative or imperative.**

> **Rules fail not because models refuse them, but because models reinterpret them.**

**責任邊界**
> **BRA 描述的是「AI 在決策時必須如何行為」，而不是「這個行為的治理結論是什麼」。**

> **Ruleset defines a decision rule lens, not a set of executable behaviors.**

### 8.2 設計原則

**Rule 作為治理原子**
> **There is no governance without a rule.
> There is no rule without a policy or constraint.**

A rule is the **smallest unit that carries governance meaning**.

**Rule 的 Mental Model**
> A **single constrained sentence**,
> written so that AI has **less room to misinterpret it**,
> while humans and governance systems can still reason about it.

**Struct Semantics 的作用機制**
> **LLM 不是被「命令」，而是被關在一個比較小的語意房間裡。**

不是增加「命令力」，而是減少「自由度」：
- WHY (intent) → 限縮「這條是用來幹嘛的」
- WHO (roles) → 限縮「誰應該服從」
- WHAT (applies_to) → 限縮「哪類輸出相關」
- WHEN (context) → 限縮「什麼情境下 relevant」
- WHAT HAPPENS (effect) → 限縮「違反的語意後果」

**Rule 規範性核心的獨占性**
> **Policy 或 Constraint 定義行為。
> 其餘一切皆在解釋背景。**

其他欄位不得：
- 強加強制性行為
- 覆寫或削弱 Policy 或 Constraint
- 引入替代解釋

### 8.3 BRA 的邊界

**BRA 負責**
```
✅ 定義 Rule（開發階段）
✅ 定義 NNL / RNL
✅ 定義 Ruleset 分類
✅ 定義 Governance Metadata
✅ 定義 Behavior Category
✅ Risk Potential（先驗評估）
✅ Rule-level Boundary（靜態宣告）
```

**BRA 不負責**
```
❌ Runtime Enforcement（BCM）
❌ Risk Signal Detection（BCM）
❌ Evidence Generation（BCM）
❌ Execution Boundary Binding（Boundary Architecture）
❌ Validation（External Tool）
❌ Schema Management（External Tool）
```