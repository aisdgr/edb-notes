# BRA 架構生態系統 - 內容分類索引

**日期**：2026-03-19

---

## 架構範圍總覽

### 核心架構

```
BRA (Behavior Rule Architecture)
  ├─ 主要架構：Rule Language + Rule Architecture + Governance Asset
  ├─ 規則重心：開發階段（Development Stage）
  └─ 範圍：僅有架構面（不含執行面）

BCM (Behavior Control Model)
  ├─ 執行面和實作
  ├─ Runtime Enforcement
  ├─ Risk Signal Detection
  └─ Evidence Generation

Boundary Architecture
  ├─ 執行範疇綁定
  ├─ Runtime Context Binding
  └─ Behavior Scope Binding

External Tools
  ├─ Validation Tools
  ├─ Schema Management
  ├─ RNL Tools
  └─ GES（Evidence Specification）
```

---

## NOTES.md 內容分類

### 1. BRA 核心素材（BRA Core）

#### 1.1 語言層（Rule Language Layer）
- **1.1 NNL 核心理念** ✅ BRA
- **1.2 RNL 核心特性** ✅ BRA

#### 1.2 規則架構層（Rule Architecture Layer）
- **1.3 Rule 結構設計** ✅ BRA
  - Rule = ⟨RNL, Metadata, Boundary_Declaration⟩
  - Policy vs Constraint 區分
  - 規範性核心
- **1.6 Governance Metadata 設計** ✅ BRA
- **1.7 devSCR 架構核心原則** ✅ BRA
- **1.8 Struct Semantics 的作用機制** ✅ BRA

#### 1.3 治理資產層（Governance Asset Layer）
- **1.4 Ruleset 概念設計** ✅ BRA
- **1.10 核心金句** ✅ BRA
- **1.11 Rule Specification Guide（RSG）精華** ✅ BRA
- **1.12 來自 RuleSpecification.md 的補充素材** ✅ BRA

---

### 2. Boundary Architecture 素材

- **1.5 Boundary 的詳細設計** ⚠️ **Boundary Architecture**
  - Boundary 的目的
  - Boundary 範例
  - 靜態宣告 vs 動態綁定

**決策相關**：
- **決策 11：Boundary 採用雙層設計（2.11）** ⚠️ **Boundary Architecture**
  - Rule-level Boundary（靜態宣告）
  - Execution Boundary（動態綁定）

---

### 3. BCM（Behavior Control Model）素材

**風險管理**：
- **決策 3：採用三層風險模型（2.3）** ⚠️ **BCM**
  - Risk Potential → BRA（先驗評估）
  - Risk Signal → **BCM**（執行階段偵測）
  - Realized Risk → Governance / Audit

**執行控制**：
- **決策 5：Risk Trigger（2.5）** ⚠️ **BCM**
  - Risk Trigger = dynamic runtime control
  - **不納入 BRA**（移至 BCM）
  - BRA = static rule definition

**Evidence 生成**：
- **決策 12：GES（2.12）** ⚠️ **BCM / External**
  - Evidence 結構定義
  - **不納入 BRA 核心**（未來工作）
  - BRA enables evidence generation but does not define evidence structure

---

### 4. 外部工具與實作

**驗證工具**：
- **1.9 Validation 三層模型** ❌ **External Tool**
  - Artifact Structure Validation
  - RNL Conformance Validation
  - Authoring Quality Advisory
- **決策 7：Validation（2.7）** ❌ **External Tool**
  - **不屬 BRA**（外部工具層）

**Schema 管理**：
- **決策 8：Schema Metadata（2.8）** ⚠️ **External Tool / BRA**
  - Optional + non-binding
  - Schema metadata MUST NOT be required for rule execution

**RNL 工具**：
- **決策 14：RNL/RNL 規範（2.14）** ⚠️ **BRA / External Tool**
  - 定義「輕規範」（不 formal grammar）
  - MUST / MUST NOT only
  - 禁止模糊詞

**其他外部工具**：
- **決策 6：Rule Relations（2.6）** ⚠️ **Extension**
  - **不放核心**（僅作 extension）
  - Rule system → constraint graph system

---

### 5. BRA 治理決策（高優先級）

- **決策 1：RNL 僅保留 MUST / MUST NOT（2.1）** ✅ BRA
- **決策 2：保留 execution_view（選填）（2.2）** ✅ BRA
- **決策 3：Risk Potential（2.3）** ✅ BRA（先驗部分）
- **決策 4：Attribute vs Imperative（2.4）** ✅ BRA
- **決策 9：Wording 最小調整（2.9）** ⚠️ BRA（小幅）
- **決策 10：Category vs Application Ruleset（2.10）** ✅ BRA
- **決策 11：Boundary 雙層設計（2.11）** ⚠️ Boundary Architecture
- **決策 13：Ruleset 意識性（2.13）** ✅ BRA

---

### 6. 跨架構內容

**核心金句**（適用於所有架構）：
- **1.10 核心金句** ✅ **Cross-Architecture**
  - "If it is injected, it is execution. If it is an attribute, it is governance."
  - "Governance does not depend on what is described, but on whether it is declarative or imperative."
  - "Rules fail not because models refuse them, but because models reinterpret them."
  - "BRA 應該只負責『定義行為約束』，而不是『執行、判斷、治理或驗證』。"

**決策摘要**（跨架構）：
- **1.13 決策建議彙整** ✅ **Cross-Architecture**

---

## 架構責任分離原則

### BRA 的責任
```
✅ 定義 Rule（開發階段）
✅ 定義 NNL / RNL
✅ 定義 Ruleset 分類
✅ 定義 Governance Metadata
✅ 定義 Behavior Category
```

### BRA 的邊界（不包含）
```
❌ Runtime Enforcement（BCM）
❌ Risk Signal Detection（BCM）
❌ Evidence Generation（BCM）
❌ Execution Boundary Binding（Boundary Architecture）
❌ Validation（External Tool）
❌ Schema Management（External Tool）
```

### BCM 的責任
```
✅ Runtime Enforcement
✅ Risk Signal Detection
✅ Evidence Generation
✅ Constraint（MUST NOT）的即時執行
✅ Policy（MUST）的事後驗證
```

### Boundary Architecture 的責任
```
✅ Execution Boundary Binding
✅ Runtime Context Binding
✅ Behavior Scope Binding
✅ 動態範疇管理
```

### External Tools 的責任
```
✅ Validation
✅ Schema Management
✅ RNL Tools
✅ Evidence Structure Definition（GES）
```

---

## 論文寫作指引

### 第 5 章：Rule Language Layer（BRA）
- 1.1 NNL 核心理念
- 1.2 RNL 核心特性

### 第 6 章：Rule Architecture Layer（BRA）
- 1.3 Rule 結構設計
- 1.6 Governance Metadata 設計
- 1.7 devSCR 架構核心原則
- 1.8 Struct Semantics 的作用機制

### 第 7 章：Governance Asset Layer（BRA）
- 1.4 Ruleset 概念設計
- 1.11 Rule Specification Guide

### 第 8 章：Extensions and Integration（Boundary Architecture + BCM）
- 1.5 Boundary 的詳細設計（Boundary Architecture）
- Risk Signal Detection（BCM）
- Runtime Enforcement（BCM）
- Evidence Generation（BCM）

### 第 9 章：Implementation and Tools（External）
- 1.9 Validation 三層模型
- Schema Management
- RNL Tools

### 第 10 章：Discussion（Cross-Architecture）
- 1.10 核心金句
- 架構責任分離
- Policy vs Constraint 差異

---

## 待處理事項

### 需要從 NOTES.md 移出的內容
1. **Boundary 詳細設計（1.5）** → 移至 Boundary Architecture 文件
2. **Risk Trigger（決策 5）** → 移至 BCM 文件
3. **Validation（1.9）** → 移至 External Tools 文件
4. **GES（決策 12）** → 移至 BCM 或 External 文件

### 需要新增的文件
1. **BCM.md**：Behavior Control Model 設計
2. **Boundary_Architecture.md**：Boundary Architecture 設計
3. **External_Tools.md**：外部工具整合

---

**最後更新**：2026-03-19
