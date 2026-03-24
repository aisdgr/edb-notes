# BRA & BCM：Claude 對話筆記

**來源：** Claude 對話（2026/3/16 – 3/20）
**主題：** BRA 論文價值評估、概念架構演進、章節結構設計

---

## BRA 論文價值與定位

### 初始評估

BRA 具備論文價值，理由：
1. 定義完整：包含多層次語言定義（NNL 到 Doc），形成完整語言家族
2. 概念創新：以結構化約束取代隱性詮釋，解決 AI 行為控制問題
3. 實用價值：Rule Catalog 展現實際應用
4. 擴展性：預留未來延伸空間
5. 實證基礎：源自真實 AI 專案經驗

不足之處：缺少應用實例、理論基礎薄弱、實證驗證需加強

### 論文定位

BRA 應定位為 **Engineering Architecture Paper**，非 Prompt Engineering Paper。

核心敘事：
```
AI execution behavior lacks rule architecture.
```

BRA 控制的是 **AI 行為**，不是 prompt。

### 核心論點（三句話）

1. Prompt optimization does not control behavior.
2. Policies are not executable.
3. BRA introduces rule-based behavior control.

---

## BRA 與 AA、VSS、Boundary 的比較

### 四���定位

| 理論 | 角色 |
|------|------|
| AA | artifact identity（所有產出基礎） |
| VSS | intent structure（規格基礎） |
| Boundary | execution scope（單次執行時選取範圍） |
| BRA | behavior constraint（AI 決策行為規範） |
| BCM | governance control |

整體體系：`AA → VSS → Boundary → BRA → BCM → DA → DR`

### Boundary 在 AA 與 BRA 中的差異

- AA：透過 Anchors 定義決策範圍，傾向「事後治理」
- BRA：透過 Rule 定義行為邊界，強調「事前治理」

### BRA 的獨立性

BRA 可以在沒有 VSS 和 Boundary 的情況下獨立成立。VSS 和 Boundary 為可選整合層，用於提高精確性但非必要。

最小可行架構只需四個元素：NNL、RNL（原 CNL）、Rule、Ruleset。

---

## 概念架構演進

### CNL → RNL 改名

將 Controlled Natural Language (CNL) 改為 Rule Natural Language (RNL)，更準確反映其作為規則描述語言的特性。

### Policy vs Constraint 的設計演進

早期設計使用 "Constraint" 統稱所有規範語句。基於實作發現：

| 維度 | MUST（Policy） | MUST NOT（Constraint） |
|------|----------------|------------------------|
| 觸發可觀測性 | 無法觀測 | 可以觀測 |
| Evidence 生成 | 不會留下 | 可以留下 |
| 對 AI 的意義 | 期望性行為 | 限制性行為 |
| 強制性等級 | 中等 | 高 |

**當前設計：Rule 為頂層概念，Policy (MUST) 和 Constraint (MUST NOT) 為子概念。**

### 整體概念鏈（最終版本）

```
Language
  NNL: For Prompt
  RNL: For Rule

Rule
  Category: Decision Behavior Category
  Rule: RNL + Metadata

Governance Asset
  Library: All Rule Repository
  Ruleset: A specific set of rules
    Category Ruleset: Same Decision Behavior
    Application Ruleset: Same Application Scenario

Execution（非 BRA 範圍）
  Control: Boundary Select Decision Behavior Rule
  Execution: AI Decision Behavior Rule
  Evidence: Decision Behavior Constraint Evidence
```

### Decision Behavior Category vs Domain Category

兩者最大差別：
- Decision Behavior Category：先界定 AI 決策行為類別，再設定規則
- Domain Category：先設計 Rule，再看可應用在哪個 Domain

BRA 採用 Decision Behavior Category，是「行為先於領域」的設計。

### Ruleset 的定位

- Ruleset 是單純的規則集合體
- Category Ruleset：基於相同 Decision Behavior 下的所有 Rule
- Application Ruleset：基於某種原則設計（Workflow-based、Domain-based 或其他），可無限種，取決於產業/企業需求

### Category Ruleset 三鐵律

1. Ruleset 不得定義 Policy 或 Constraint
2. Ruleset 是「視角」不是「控制器」
3. Ruleset 必須能被多系統重用

### 正確分層

```
Language Layer → Behavior Category → Rule Layer → Asset Layer → Application Ruleset
```

三個關鍵原則：
- Composition Scope：Rule composition MUST occur only within Application Ruleset
- Category Isolation：Behavior Categories MUST NOT define or enforce Rule composition
- Atomic Rule Integrity：Each Rule MUST remain independently selectable and composable

---

## BRA 核心模型與設計原則

### 核心鏈條

```
NNL → RNL → Rule → Ruleset → Execution
```

這條鏈是 BRA 的核心創新。

### Rule 的定位

Rule 不是 Markdown，而是 structured behavior constraint。建議 YAML/JSON 格式。

Rule 內容包含：CNL（MUST / MUST NOT）、Risk、Owner、Context 等治理欄位。

Rule 範例（CODE-AR-05）：
```yaml
rule:
  id: CODE-AR-05
  execution_view: artifact_engineer
  constraint: >
    All generated artifacts MUST be addressable and locatable
    within the execution context.
  boundary:
    domain: [code]
    artifact: [source_code]
    rule_category: [artifact]
    rule_domain: [CODE]
  governance:
    version: 1.0.0
    status: draft
    owner: asdgr
    intent:
      rationale:
        - prevent_unreachable_artifacts
        - ensure_artifact_traceability
        - enable_post_execution_audit
    responsibilities:
      ai: [code_executor]
      system: [artifact_validator]
    applicability_context:
      stage: [develop]
      scope: [artifact_creation]
    governance_impact:
      level: blocking
      affected_capabilities:
        - artifact_traceability
        - auditability
        - execution_transparency
```

### Ruleset 的核心原則

**一組 Ruleset 至少要有一條 Constraint**（MUST NOT），確保行為受約束。

沒有 Constraint = 沒有行為控制。

### Rule Library 的使用方式

Rule Library ≠ 全部載入。而是 purpose-based ruleset selection。

例如：
- source_code.change
- test_code.add
- test.execution

這是 BRA 與現有 rules files 最大差異。

### Rule 可重用性

Rule 是可跨產業、跨企業、跨專案共享的治理資產（governance asset），不只是 prompt 片段。

### BRA 的三個真正創新點

1. NNL / RNL 分層：prompt vs behavior
2. Ruleset 動態選取：context-driven
3. 最小 Constraint 原則：強制行為控制

簡化成一句話：
```
BRA separates intent optimization from behavior enforcement.
```

---

## 論文章節結構演進

### 最終章節結構

1. Introduction（800–1000 words）
2. Related Work（1000–1500 words）
3. Problem Statement（1000–1200 words）
4. Conceptual Model（1200–1500 words）
5. Rule Language Layer（1500–2000 words）
6. Rule Architecture Layer（2000–2500 words）
7. Behavior Execution Layer（2000–2500 words）
8. External Integration and Architecture Ecosystem
9. Discussion（1000–1500 words）
10. Conclusion（800–1000 words）

### Related Work 位置

建議放在 Methodology 之前（中間位置），理由：
- 讀者初步了解 BRA 後再回顧相關工作，比較更有針對性
- 為 BRA 的 Rule Language、Rule Architecture 設計選擇提供合理性支撐

### 敘事流程

```
Language → Rule → Execute → Extensions/Integration
```

### 論文三個關鍵章節

4. Conceptual Model、5. Rule Language Layer、6. Rule Architecture Layer 是論文核心。

### 論文必須包含的範例

```
generate test code → Ruleset: test_code.add
  Constraint: AI MUST NOT modify production code

execute tests → Ruleset: test.execution
  Constraint: AI MUST NOT modify artifacts
```

### 論文必須包含的兩張圖

1. `NNL → CNL → Rule → Ruleset → Execution`
2. `VSS → Boundary → BRA`

---

## BRA vs 現有實務方法

### 最相似的實務方法

- GitHub Copilot AGENTS.md / copilot-instructions.md
- Cursor Rules（.cursorrules / .cursor/rules/）
- Claude Code AGENTS.md

### BRA 的差異

| 面向 | 現有方法 | BRA |
|------|----------|-----|
| 規則形式 | Markdown 文字 | YAML/JSON 結構化 |
| 治理元數據 | 無 | Risk、Owner、Context 等 |
| 組合方式 | 全部載入 | Purpose-based 動態選取 |
| 最小約束 | 無要求 | 至少一條 Constraint |
| 語言分層 | 無 | NNL / RNL 分層 |

### Grok 的「已有類似、創新不大」評論的回應

Grok 的觀點有局限性。雖然相似方法存在，BRA 在以下方面有顯著差異：
- 理論深度：完整行為治理理論框架 vs 實踐經驗總結
- 系統性：語言 → 規則 → 治理資產的完整體系
- 實用性：結構化 Rule + 動態 Ruleset 選取
- 擴展性：模組化設計，可適應不同領域
- 平台無關性：不限特定 AI 工具

---

## Rule Library 與 GitHub 引用

### 可否在論文中引用 GitHub 上的 Rule Library

可以，但須注意：
- 聚焦理論創新，Rule Library 作為佐證
- 選擇性展示關鍵 Rule，非逐一羅列
- 強調 Rule Library 的示例作用
- 避免論文變成規格說明書

---

## Appendix 規劃

### Rule List（附錄 A）

涵蓋 7 大類別：Isolation、Boundary、Inference、Logging、Structural、Test、Traceability。

### Rule-Ruleset Mapping Matrix（附錄 B）

以矩陣形式展示 Rule 在不同 Ruleset 中的應用，直觀體現 Ruleset 動態組合機制。

---

## 待討論項目

### 14 個需決策的衝突點

#### 優先處理

1. **RNL 關鍵字範圍**：是否包含 SHOULD / SHOULD NOT
   - 建議：僅保留 MUST / MUST NOT，SHOULD 視為 NNL 範疇

2. **Risk Potential 三層風險模型**
   - 三層：Risk Potential（先驗）→ Risk Signal（執行中）→ Realized Risk（稽核）
   - 建議採用 devSCR 設計，在 Metadata 中使用 risk_potential

3. **Category vs Application Ruleset 邊界**
   - 建議明確區分：Category 專注分類，Application Ruleset 專注組合

4. **Boundary 的位置**
   - Rule 內部 vs 外部層
   - 建議兩層 boundary：Rule 內部簡化欄位 + 外部動態綁定

#### 次要處理

5. **execution_view**
   - 解釋視角，非執行角色
   - 建議採用 RSG.md 定義，作為選填欄位

6. **Attribute vs Imperative 分離**
   - Attribute 為治理元素，Imperative 為執行指令
   - 建議採用 x-meta 標註語義角色

7. **Risk Trigger 與 Required Behavior**
   - Risk trigger ≠ Risk judgment，Risk trigger ⇒ Required behavior

8. **RNL 的 CNL 語法規範**
   - 建議定義簡化 CNL 語法，人類可讀優先

#### 可選處理

9. **Rule Relations**（互斥、依賴、分組）：暫不引入
10. **Validation 三層模型**：屬於工具層
11. **Schema Metadata**：移至註解
12. **Wording 建議**（Intent → governance_purpose 等）：最小侵入
13. **GES (Governance Evidence Specification)**：未來工作方向
14. **Ruleset 意識性**：Rule 不感知所屬 Ruleset

### Ruleset 與 Rule 的關係

| Aspect | Rule | Ruleset |
|--------|------|---------|
| Normative | Yes | No |
| Atomic | Yes | No |
| Executable | No | No |
| Governance Scope | Local | Aggregated |
| Versioned | Yes | Yes |

核心結論：A Ruleset is greater than one Rule, but less than a governance system.

---

## 概念調整��後比較

### 調整前

- Rule Category 定位不夠明確
- Ruleset 內涵和組成原則未詳細說明
- Application Ruleset 僅限 Workflow-based 和 Domain-based

### 調整後

- Decision Behavior Category 明確定位（行為先於領域）
- Ruleset 定義為單純規則集合體
- Application Ruleset 設計原則無限擴展
- Category Ruleset 與 Application Ruleset 邊界清晰

### 與現有方法比較

| 維度 | 現有方法 | BRA |
|------|----------|-----|
| 系統性 | 單一層面 | 多層次框架 |
| 行為導向 | 基於領域/任務 | 基於決策行為 |
| 靈活性 | 適用性窄 | Application Ruleset 可無限組合 |
| 治理資產化 | 規則管理不足 | Rule Library 可管理、共享、重用 |
| 執行閉環 | 關注制定 | 從規則定義到 Evidence 全流程 |

---

## DevOps 與 Runtime 延伸

BRA 可從軟體開發延伸至 DevOps 和 Runtime Stage：

- **DevOps Stage**：VV、TI、LG 類別 Rule 融入 CI/CD 流程
- **Runtime Stage**：BD、IS、IC、EC 類別 Rule 實現動態監控和實時控制

---

## 整體評價

### 學術價值

- 新的 AI 行為治理範式
- 完整理論框架和層次結構
- 跨學科視角（NLP、軟體工程、AI 治理）

### 工業影響

- 系統化行為治理方法
- 降低 AI 行為風險
- 低部署成本（非侵入式設計）

### 實際應用

- 標準化語言（YAML/JSON）實現
- 清晰實現藍圖
- 模組化設計，易擴展

---

## 編輯檢查

- [x] 依原始對話時間排序
- [x] 保留所有有意義內容
- [x] 只去除噪音，非刪除語意
- [x] 保留推論過程與被推翻的推論
- [x] 將對話痕跡改寫為可閱讀的筆記結構
- [x] 避免重複敘述同一內容
