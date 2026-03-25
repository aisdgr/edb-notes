# Series Goal

## Core Theme
Behavior Rule Architecture（BRA）是一套 AI 執行行為規範工程框架，核心主軸為：**當 AI 進入執行階段，工程的可預期性正在流失，而解方不是更好的 prompt，而是結構化的行為治理架構。**

BRA separates intent optimization from behavior enforcement — 這是整個系列的唯一基調。

## Target Transformation
讀者看完後會：
- 理解 prompt 的三個根本上限（行為前驗證、行為邊界鎖定、行為責任歸屬），以及為什麼 prompt engineering 無法突破這些限制
- 建立「語義分離」的認知框架：意圖表達（NNL）與行為強制（RNL）是兩種不同的工程問題，不應混雜在同一層
- 理解 Rule 作為治理資產的設計邏輯：Policy (MUST) 與 Constraint (MUST NOT) 的本質差異、Decision Behavior Category 先於 Domain 的設計哲學
- 接受 Ruleset 是「透鏡」而非「控制器」的三鐵律思維
- 改變認知：AI 行為治理不是事後稽核，而是從開發階段就嵌入的結構性工程問題（治理必須左移）

## Scope
- 包含：
  - 問題基礎：AI 行為治理的必要性（不確定性三層級、prompt 三上限、權責錯位）
  - BRA 核心架構概覽與三層設計
  - Rule Language Layer：NNL 與 RNL 的語義分離
  - Rule Architecture Layer：Rule 結構、Policy vs Constraint、Decision Behavior Category
  - Governance Asset Layer：Rule Library、Rule Category、Ruleset 三鐵律
  - 設計原則：Attribute vs Imperative 分離、Struct Semantics
  - 外部整合層：Boundary Architecture（兩層設計）、BCM（治理框架互補）
  - 與現有方法的比較差異（Prompt Engineering、Guardrails、Policy-as-Code）
  - CODE Rule Catalog 作為實證案例
- 不包含：
  - devSCR 的 schema 實作細節與語法
  - AISL 的語言模型設計
  - BCM 的完整治理循環細節（PDCA、Risk Signal Detection）
  - Rule Relations（互斥、依賴、分組）— 延伸功能
  - GES（Governance Evidence Specification）— 未來工作
  - Validation 三層模型的工具實作
  - 論文撰寫策略與學術發布計畫
  - 命名演進過程（CNL → RNL、BNL/DBNL 淘汰分析）— 內部設計歷史

## Core Concepts
- NNL（Normative Natural Language）：意圖優化語言，减少 AI 猜測空間
- RNL（Rule Normative Language）：行為強制語言，僅使用 MUST / MUST NOT
- 語義分離（Semantic Separation）：意圖表達 ≠ 行為約束
- Rule：結構化治理產出物 = ⟨RNL, Governance Metadata, Boundary_Declaration⟩
- Policy (MUST) vs Constraint (MUST NOT)：期望性 vs 限制性、不可觀測 vs 可觀測
- Decision Behavior Category：先定義行為再衍生規則（行為先於領域）
- Ruleset：決策約束透鏡，三鐵律（不定義規範、是視角非控制器、必須可重用）
- Rule Library：跨階段共享的治理資產
- Attribute vs Imperative 分離：描述性屬性 ≠ 執行指令
- Boundary Architecture 兩層設計：Rule-level（語義適用）+ Execution-level（Runtime 綁定）
- Governance 左移：治理從事後稽核移至開發階段

## Narrative Strategy
- **先揭露問題不可逆性**：AI 進入執行後，工程失去責任 — prompt 的天花板、效率放大但權責消失、治理太晚就來不及
- **再提出架構框架**：BRA 的三層設計，從語言層（NNL/RNL 語義分離）到架構層（Rule 結構、Policy vs Constraint）到資產層（Ruleset 三鐵律、Rule Library）
- **然後展示設計原則**：Attribute vs Imperative 分離、Struct Semantics 的有效性來源、三層風險模型
- **最後收斂為行動認知**：治理必須左移，Rule 作為治理資產可跨階段重用，Categories describe behavior; Rules enforce constraints; Applications decide composition

## Platform Strategy
- Medium：完整論述，系列文形式，每篇獨立可讀但整體構成完整認知鏈
  - 受眾：軟體工程師、AI 工程師、技術架構師、技術管理者
  - 風格：工程論述 + 問題揭露，避免純學術語氣，保持實務感
  - 篇幅：每篇 1500–2500 字，系列 3–5 篇
