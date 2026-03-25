# Medium Index — behavior-rule-architecture

## 平台資訊
- 平台：Medium
- 特色：系統性觀點輸出，核心內容承載平台，受眾為中高階工程師、系統架構師、技術研究者
- 風格：工程論述 + 問題揭露，章節式結構（H2/H3），解釋型與理性語氣，避免過度學術化，每篇 1500–2500 字

---

## 文章列表

### P1：When AI Enters Execution, Engineering Starts Losing Responsibility
- 目標: 建立治理需求認知 — 讀者理解 prompt 三上限與治理左移的必要性，接受「治理不是事後稽核，而是開發階段的結構性工程問題」
- 範圍: 問題基礎、與現有方法的比較差異；涵蓋 Core Concepts：Governance 左移
- 內容大綱:
  - LLM 不確定性與工程可預期性的根本衝突
    - 參考 OUTLINE：[behavior-rule-architecture.md](../../../../outline/behavior-rule-architecture.md) § 問題基礎：AI 行為治理的必要性
  - Prompt 三上限：無法行為前驗證、無法行為邊界鎖定、無法行為責任歸屬
    - 參考 OUTLINE：[behavior-rule-architecture.md](../../../../outline/behavior-rule-architecture.md) § 問題基礎：AI 行為治理的必要性
  - 效率放大但權責消失：AI 回答了 How，Why 正在消失
    - 參考 OUTLINE：[behavior-rule-architecture.md](../../../../outline/behavior-rule-architecture.md) § 問題基礎：AI 行為治理的必要性
  - 治理失敗不在控制不夠，而在治理發生得太晚
    - 參考 OUTLINE：[behavior-rule-architecture.md](../../../../outline/behavior-rule-architecture.md) § 問題基礎：AI 行為治理的必要性
  - 與現有方法的比較：Prompt Engineering、Guardrails、Policy-as-Code 為何不夠
    - 參考 OUTLINE：[behavior-rule-architecture.md](../../../../outline/behavior-rule-architecture.md) § 與現有方法比較

---

### P2：Two Languages, One Problem — NNL, RNL, and Why Semantic Separation Matters
- 目標: 建立語義分離認知框架 — 讀者理解意圖表達與行為約束是兩種不同的工程問題，以及 BRA 的核心設計動機
- 範圍: BRA 架構概覽、Rule Language Layer；涵蓋 Core Concepts：NNL、RNL、語義分離
- 內容大綱:
  - BRA 核心定位：AI Execution Behavior Normative Engineering Framework
    - 參考 OUTLINE：[behavior-rule-architecture.md](../../../../outline/behavior-rule-architecture.md) § BRA 核心架構概覽
  - 三層內部架構與核心概念鏈
    - 參考 OUTLINE：[behavior-rule-architecture.md](../../../../outline/behavior-rule-architecture.md) § BRA 核心架構概覽
  - NNL：意圖優化語言 — Important things should not be left for AI to guess
    - 參考 OUTLINE：[behavior-rule-architecture.md](../../../../outline/behavior-rule-architecture.md) § NNL（Normative Natural Language）
  - RNL：行為強制語言 — Not making AI freer, but making AI harder to deviate
    - 參考 OUTLINE：[behavior-rule-architecture.md](../../../../outline/behavior-rule-architecture.md) § RNL（Rule Normative Language）
  - 語義分離：解決「意圖與約束混雜」的根本問題
    - 參考 OUTLINE：[behavior-rule-architecture.md](../../../../outline/behavior-rule-architecture.md) § NNL 與 RNL 的語義分離

---

### P3：Rule as a Governance Asset — Structure, Classification, and the Three Iron Laws
- 目標: 理解 Rule 作為治理資產的設計邏輯 — 讀者掌握 Policy vs Constraint 差異、Decision Behavior Category 哲學、Ruleset 三鐵律
- 範圍: Rule Architecture Layer、Governance Asset Layer；涵蓋 Core Concepts：Rule、Policy vs Constraint、Decision Behavior Category、Ruleset 三鐵律、Rule Library
- 內容大綱:
  - Rule 結構：四個核心構件與設計原則（Norm Over Control, Declarative Not Procedural）
    - 參考 OUTLINE：[behavior-rule-architecture.md](../../../../outline/behavior-rule-architecture.md) § Rule 結構（Structured Governance Artifact）
  - Policy (MUST) vs Constraint (MUST NOT)：期望性 vs 限制性、不可觀測 vs 可觀測的本質差異
    - 參考 OUTLINE：[behavior-rule-architecture.md](../../../../outline/behavior-rule-architecture.md) § Policy (MUST) vs Constraint (MUST NOT)
  - Decision Behavior Category：先界定 AI 決策行為，再衍生規則
    - 參考 OUTLINE：[behavior-rule-architecture.md](../../../../outline/behavior-rule-architecture.md) § Decision Behavior Category
  - Rule Library 與 Rule Category：治理資產化與共享重用
    - 參考 OUTLINE：[behavior-rule-architecture.md](../../../../outline/behavior-rule-architecture.md) § Rule Library
    - 參考 OUTLINE：[behavior-rule-architecture.md](../../../../outline/behavior-rule-architecture.md) § Rule Category
  - Ruleset 三鐵律與正確分層組合原則
    - 參考 OUTLINE：[behavior-rule-architecture.md](../../../../outline/behavior-rule-architecture.md) § Ruleset（純規則集合體）
    - 參考 OUTLINE：[behavior-rule-architecture.md](../../../../outline/behavior-rule-architecture.md) § Ruleset 三鐵律
    - 參考 OUTLINE：[behavior-rule-architecture.md](../../../../outline/behavior-rule-architecture.md) § 正確分層與組合原則

---

### P4：Governance Must Shift Left — Design Principles, Boundaries, and the CODE Rule Catalog
- 目標: 展示設計原則的實踐力、收斂為行動認知 — 讀者接受治理左移、理解 Rule 作為跨階段治理資產的實務價值
- 範圍: 設計原則、外部整合層、CODE Rule Catalog 實證；涵蓋 Core Concepts：Attribute vs Imperative 分離、Boundary Architecture 兩層設計、Governance 左移
- 內容大綱:
  - Attribute vs Imperative 嚴格分離：If it is injected, it is execution. If it is an attribute, it is governance
    - 參考 OUTLINE：[behavior-rule-architecture.md](../../../../outline/behavior-rule-architecture.md) § Attribute vs Imperative 嚴格分離
  - Struct Semantics：有效性來自減少自由度，而非增加命令力
    - 參考 OUTLINE：[behavior-rule-architecture.md](../../../../outline/behavior-rule-architecture.md) § Struct Semantics
  - Boundary Architecture 兩層設計：Rule-level 語義適用 + Execution-level Runtime 綁定
    - 參考 OUTLINE：[behavior-rule-architecture.md](../../../../outline/behavior-rule-architecture.md) § Boundary Architecture
  - BCM 互補定位：BRA = 靜態結構，BCM = 動態治理循環
    - 參考 OUTLINE：[behavior-rule-architecture.md](../../../../outline/behavior-rule-architecture.md) § BCM（Behavior Control Model）
  - CODE Rule Catalog 實證：126 條規則如何落地 BRA 概念
    - 參考 OUTLINE：[behavior-rule-architecture.md](../../../../outline/behavior-rule-architecture.md) § CODE Rule Catalog
  - 收斂：治理左移的行動認知 — Categories describe behavior; Rules enforce constraints; Applications decide composition
    - 參考 OUTLINE：[behavior-rule-architecture.md](../../../../outline/behavior-rule-architecture.md) § 核心金句集

---

## 覆蓋範圍

| 概念 | 貼文 |
| ---- | ---- |
| NNL（Normative Natural Language） | P2 |
| RNL（Rule Normative Language） | P2 |
| 語義分離（Semantic Separation） | P2 |
| Rule（結構化治理產出物） | P3 |
| Policy (MUST) vs Constraint (MUST NOT) | P3 |
| Decision Behavior Category | P3 |
| Ruleset 三鐵律 | P3 |
| Rule Library | P3 |
| Attribute vs Imperative 分離 | P4 |
| Boundary Architecture 兩層設計 | P4 |
| Governance 左移 | P1, P4 |
