# devSCR 核心概念筆記

> Development-Stage Structured Constraint Representation
> 開發階段結構化約束表示

---

## devSCR 定義與定位

### 核心定義

**devSCR** = 用以在開發階段，結構化描述「AI 在做決策／生成時應遵守的約束」的表示層。

### 定位

- 不是 Runtime Governance
- 不是 Organizational Governance
- 是 **Decision Behavior Governance 在「開發階段」的落地表示**

### 責任切割

| 層級          | 責任                                             |
| ------------- | ------------------------------------------------ |
| DBG           | 治理哲學（What is governed）                     |
| DCL           | 約束架構（Where governance exists）              |
| **devSCR**    | **開發階段約束的結構化表示（How, at dev time）** |
| Runtime / Ops | 行爲觀測、證據、結果標示                         |

### 關鍵原則

> **devSCR 描述的是「AI 在決策時必須如何行爲」，而不是「這個行爲的治理結論是什麼」。**

---

## 四個核心構件

### NNL (Natural Normative Language)

**角色：** 人類可讀、審閱、討論用的規範語言

**邊界：**
- 不負責可執行
- 不負責嚴格判斷
- 作爲 CNL / Rule 的**語義來源**

> NNL = 意圖與價值的入口

### CNL (Controlled Normative Language)

**角色：** 可約束、低歧義的行爲描述

**特性：**
- 比自然語言更嚴格
- 比程式碼更具語意彈性
- 非執行層，但可被工具驗證

> CNL = 可治理的行爲語意層

### Rule

**角色：** 單一決策約束單元（Decision Constraint Atom）

**正確內容：**
- intent（WHY / WAY）
- roles（WHO）
- applies_to（WHAT）
- behavior / constraint（HOW）→ 多半用 CNL 表達

**不應該包含：**
- organizational risk
- compliance status
- audit decision
- overall governance score

> Rule = 單一決策約束單元

### Ruleset

**角色：** 一組「針對某類開發活動」的約束集合

**特性：**
- 關注「開發決策品質與穩定性」
- 不等於治理結果
- 是治理**存在的前提材料之一**

> Ruleset = 開發階段的決策約束包

---

## Risk 處理原則

### 三種風險的區別

| 類型                         | 定義                                         | 所在層                       |
| ---------------------------- | -------------------------------------------- | ---------------------------- |
| **Risk Potential**           | 規則「一旦被違反或被忽略」可能造成多大的影響 | devSCR（先驗評估）           |
| **Risk Signal / Exposure**   | 執行過程中出現了哪些風險徵象                 | execution / observation      |
| **Realized Risk / Severity** | 實際造成的後果與嚴重性                       | audit / governance judgement |

### 正確處理方式

**不要在 devSCR 中放：**
- `risk: high | medium | low`（治理判斷）
- `severity:`（結果評級）
- `compliance:`（合規結論）
- `approval_required:`（流程裁決）

**應該在 devSCR 中放：**
```yaml
risk_potential:
  impact_score: 4        # 1–5
  rationale: affects_release_safety
```

> **Risk attribute ≠ execution parameter**

---

## Attribute vs Imperative 分離原則

### 核心原則

> **凡是 attribute 形式存在的信息，都屬於 governance element，而非 execution directive。**

### 正確分界

| 類型                    | 角色                 | 受衆                            |
| ----------------------- | -------------------- | ------------------------------- |
| **Attribute**           | 可被治理的描述性事實 | governance machinery / 人類審查 |
| **Imperative Behavior** | 可被執行的行爲要求   | AI agent                        |

### 示例

**錯誤（治理語言）：**
```yaml
risk: high
severity: critical
```

**正確（attribute 化）：**
```yaml
attributes:
  risk_potential:
    impact_score: 4
    rationale: affects_release_safety
  behavior_characteristics:
    autonomy_level: low
    requires_human_attention: true
```

> **Anything expressed as an attribute is governed; anything expressed as behavior is executed.**

---

## Execution Boundary 原則

### 真正進入 execution 的只有三樣

> **Execution = CNL × execution_role × applies_to**

### 三個要素

1. **CNL** — 唯一可被 AI agent「理解爲行爲要求」的語意層（MUST / MUST NOT / SHOULD、IF–THEN 行爲條件、對 decision 行爲的直接約束）
2. **execution_role** — AI 以什麼職責角色 persona 執行（SA ≠ Engineer ≠ QA，同一條 CNL 在不同 persona 下行爲結果不同，必須 injection）
3. **applies_to** — 限定約束的作用範圍（source_code / test_code / config）

### 責任對照表

| 項目                | devSCR（開發階段） | Execution / Runtime |
| ------------------- | ------------------ | ------------------- |
| risk 等級           | 不存在             | 標示結果            |
| severity            | 不存在             | 結果評估            |
| HITL                | 行爲條件           | 實際發生與否        |
| constraint          | MUST / MUST NOT    |                     |
| governance judgment |                     | 事後                |

> **If it is injected, it is execution. If it is an attribute, it is governance.**

---

## Risk Trigger 與 Required Behavior

### 核心概念

> **Risk trigger ≠ Risk judgment**
> **Risk trigger ⇒ Required behavior**

### 三層語意分離

**① 規則層：Risk Potential（先驗重要度）**
- 規則本身有多重要，用於排序、加權、門檻，不會觸發行爲

**② 觸發層：Risk Trigger（事實發生）**
- 缺乏 test
- 不確定需求來源
- 涉及敏感模組
- 跨越權責邊界

**③ 行爲層：Required Behavior（必須行爲）**
- 一旦風險被觸發，AI 不得自由行爲，而必須進入指定行爲模式

### 正確定義方式

**不要（結果式）：**
```yaml
risk: high
action: block
```

**正確（行爲要求）：**
```yaml
rule:
  intent: ensure_test_coverage

  triggers:
    - condition: no_test_found

  required_behavior:
    - must_request_human_review
    - must_not_finalize_output
    - must_emit_risk_flag
```

> **Risk without prescribed behavior is not governance; it is merely annotation.**

---

## Struct Semantics 的作用

### 爲什麼 Struct Semantics 有效

**不是增加「命令力」，而是減少「自由度」**

| Struct Semantic       | 對 LLM 的實際影響           |
| --------------------- | --------------------------- |
| WHY (intent)          | 限縮「這條是用來幹嘛的」    |
| WHO (roles)           | 限縮「誰應該服從」          |
| WHAT (applies_to)     | 限縮「哪類輸出相關」        |
| WHEN (context)        | 限縮「什麼情境下 relevant」 |
| WHAT HAPPENS (effect) | 限縮「違反的語意後果」      |

> LLM 不是被「命令」，而是被關在一個比較小的語意房間裡。

### 對 GPT-4.1+ 特別有效的原因

模型會把結構當成「語意邊界線」，而不是只是資料格式：
- YAML key ≈ semantic anchor
- 層級 ≈ priority / scope
- capsule ≈ interpretation domain

### 幫助有限的情境

1. 模型被硬性 override（system prompt 強制）
2. 約束本身含糊（CNL 語意不清）
3. 完全未投影到 prompt（struct semantics 沒有進入 prompt）

> **schema + 正確投影，才會產生實質約束效果**

> **Constraints fail not because models refuse them, but because models reinterpret them.**

---

## 架構驗證：四大目標達成

| 目標                    | 是否成立 | 關鍵原因                   |
| ----------------------- | -------- | -------------------------- |
| LLM strong constraint   | ✔        | 限縮解讀空間，而非強迫行爲 |
| Governance              | ✔        | 描述性、多視角、可審計     |
| Anti-LLM drift          | ✔        | 語意邊界結構化、可演進     |
| Abstraction application | ✔        | AISCR 抽象 + devSCR 實例   |

> **This architecture achieves strong LLM constraints, effective governance, and drift resistance not by embedding execution logic, but by structurally stabilizing interpretation boundaries through abstract, declarative constraint representation.**

---

## Wording 建議

### 總原則

> **Avoid action-oriented and execution-suggestive terms in governance-facing fields. Reserve normative force only for constraint-specific wording.**

### 建議調整

| 原詞    | 建議                                           | 原因                                     |
| ------- | ---------------------------------------------- | ---------------------------------------- |
| Intent  | `governance_purpose` / `policy_intent`         | 避免 user intent / execution goal 的誤解 |
| Roles   | `responsibility_roles` / `governance_role`     | 明確是責任歸屬，不是 runtime persona     |
| Context | `applicability_context` / `governance_context` | 避免 if 條件背景的誤解                   |
| Effect  | `governance_impact` / `policy_implication`     | 避免執行 / side-effect 的誤解            |

### 最小侵入建議

用 `x-meta.semantic_role` 標記語意角色，不強制改欄位名：
```yaml
intent:
  x-meta:
    semantic_role: governance_viewpoint

roles:
  x-meta:
    semantic_role: responsibility_assignment

context:
  x-meta:
    semantic_role: applicability_context

effect:
  x-meta:
    semantic_role: governance_impact
```

---

## Rule Schema 結構

### Canonical Rule 結構

```yaml
rule:
  id: GEN-ADD-JWT-AUTHENTICATION

  # Execution View
  execution_view: security_engineer

  # Constraint (唯一 normative)
  constraint: >
    Generated code MUST include JWT-based authentication
    when introducing new protected API endpoints.

  # Boundary (WHERE / SCOPE)
  boundary:
    change_type:
      - new_code
    feature_scope:
      - authentication
    security_mechanism:
      - jwt

  # Governance
  governance:
    version: "1.0.0"
    status: active
    owner: security_architecture_team

    intent:
      rationale:
        - enforce_authentication_baseline

    responsibilities:
      ai:
        - code_generator
      human:
        - security_reviewer

    applicability_context:
      scenario:
        introducing_protected_endpoint:
          risk: security
          severity: high

    governance_impact:
      outcome:
        classification: authentication_enforced
      evidence_expected:
        - jwt_validation_logic_present
```

### 關鍵要點

1. **constraint 是唯一 normative**
2. **boundary / applicability_context / governance_impact 全爲企業可自定義 map**
3. **沒有任何 IF / THEN、沒有執行流程暗示**
4. **schema meta 與 rule 本體解耦**

---

## Ruleset 概念

### 定義

> **Ruleset 定義一個決策約束透鏡（decision constraint lens），而不是一組可執行行爲。**

### 三個鐵律

**鐵律 1：Ruleset 不得定義 constraint**

不能出現：MUST / SHALL、行爲描述、IF / THEN、execution hint
只能：reference rules、描述組織與治理語意

**鐵律 2：Ruleset 是「視角」，不是「控制器」**

> 「在這個 decision context 下，我們 _考慮_ 哪些 rule」

而不是：「在這種情況下，執行這些 rule」

**鐵律 3：Ruleset 必須能被多系統重用**

- 同一組 rules 可以被不同 ruleset 使用
- 同一個 rule 可以存在於多個 ruleset

### Ruleset 結構

```yaml
ruleset:
  id: FINTECH-SECURITY-BASELINE
  profile: devSCR        # 或 AISCR / DCL

  applies_to:            # decision lens
    domain:
      - fintech
    system:
      - payment_api
    phase:
      - development

  includes:              # rule references
    - rule_id: GEN-NEW-CODE-ADD-JWT
      version: ">=1.0.0"
    - rule_id: API-MUST-HAVE-AUTH

  governance:            # governance-only metadata
    owner: security_team
    intent:
      industry:
        - fintech
      regulation:
        - iso_42001
    lifecycle:
      status: active
```

---

## Rule / Ruleset / Category 關係

### 正確分層

```
Language Layer
   ↓
Behavior Category (design basis only)
   ↓
Rule Layer (atomic constraints)
   ↓
Asset Layer
   └─ Application Ruleset (composition happens here)
```

### 三個組合原則

1. **Composition Scope** — Rule composition MUST occur only within an application ruleset
2. **Category Isolation** — Behavior categories MUST NOT define or enforce rule composition
3. **Atomic Rule Integrity** — Each rule MUST remain independently selectable and composable

### Category vs Rule vs Application

| 類型                    | 角色                              | 特性                                  |
| ----------------------- | --------------------------------- | ------------------------------------- |
| **Category**            | 行爲分類（behavior dimension）    | 不負責組合、不負責選擇             |
| **Rule**                | 單一 Decision Behavior Constraint | 一條 Rule = 一個 Category、不可再拆 |
| **Application Ruleset** | 唯一組合層                        | 決定哪些 rule 被使用                |

> **Categories describe behavior; rules enforce constraints; applications decide composition.**

> **Rule 的自由組合權，只存在於 Application，而不是 Category。**

---

## Rule Relations（規則間關係）

### 需要的關係類型

**① Mutual Exclusion（互斥）** — R4 ⊥ R5，不能同時存在於同一個 application

**② Dependency（依賴）** — R1 → R2，選擇 R1 時必須選擇 R2

**③ Grouping（分組）** — {R1, R2, R3} ∈ G_test_writing，屬於同一個功能組

### Application Ruleset 擴展

```yaml
application:
  id: test-application

  includes:
    - rule_id: R1
    - rule_id: R2
    - rule_id: R6

  relations:
    - type: exclusion
      between: [R4, R5]
    - type: dependency
      from: R1
      to: R2
```

---

## Schema 元數據處理

### 兩種做法

**做法 A（推薦給 example / public doc）**

把 schema 信息移到註解：最乾淨、最不會誤導、最適合對外公開與教學

```yaml
# ==============================================================================
# Schema: dev-scr-rule-schema
# Schema Version: 1.0.0
# Status: example
# ==============================================================================
```

**做法 B（工具用 / pipeline 用）**

保留 `meta:`，但工具可以選擇忽略

### 規範語句

> **Schema metadata is optional at the document level and MUST NOT be required for rule interpretation.**

---

## Validation 三層模型

### 1. Artifact Structure Validation

用 JSON Schema validator 檢查 YAML / JSON 基本結構，pass / fail，不碰 CNL、不碰語意。這是最低層、必須存在的 validator。

### 2. CNL Conformance Validation

檢查 `rule.constraint` 是否符合某個 CNL representation schema（用 `cnl.generate.schema.yaml`），warning 或 fail（依模式）。不是 rule 必要條件，而是 quality gate。這一層必須是「可設定 strict / advisory 模式」。

### 3. Authoring Quality Advisory

幫人類作者理解欄位對齊、舊新格式轉換、表達清晰度。結果永遠不該是 failed，只產生 warning / info。這是 AI 作爲「輔助工具」的最佳位置。

### Validation Result 結構

```yaml
summary:
  status: warning

checks:
  artifact_structure:
    status: pass

  cnl_conformance:
    status: warning
    strict: false

  authoring_advisory:
    status: warning

findings:
  - ...
```

---

## 核心金句

### 架構層級

> **If it is injected, it is execution. If it is an attribute, it is governance.**

> **Governance does not depend on what is described, but on whether it is declarative or imperative.**

> **Risk without prescribed behavior is not governance; it is merely annotation.**

### 責任邊界

> **devSCR 描述的是「AI 在決策時必須如何行爲」，而不是「這個行爲的治理結論是什麼」。**

> **Ruleset defines a decision constraint lens, not a set of executable behaviors.**

### 組合原則

> **Categories describe behavior; rules enforce constraints; applications decide composition.**

> **Category 是選擇的入口，不是關係的終點。**

> **Application 不擁有 Category，只擁有 Rule。**

---

## 實施建議

### 開發順序

1. 先定 Ruleset 概念文件（1 頁，像 rule.md）
2. 再調整 `ruleset.generate.schema.yaml`
3. 最後才考慮 ruleset validation / tooling

### Example 撰寫原則

- 一檔一 rule
- CNL 是唯一 normative
- 沒有 IF / THEN、沒有執行流程暗示
- schema meta 與 rule 本體解耦

### 品質控制

- schema + 正確投影，才會產生實質約束效果
- CNL 語意必須清晰
- 避免 governance leakage（attribute 不 injection）

---

## 編輯檢查

- 是否依原始對話時間排序：是
- 是否保留所有有意義內容：是
- 是否只去除噪音，而非刪除語意：是
- 是否保留推論過程與被推翻的推論：是（包含 governance leakage 等被推翻的做法）
- 是否將對話痕跡改寫爲可閱讀的筆記結構：是
- 是否避免重複敘述同一內容：是

---

**來源：** devSCR.manuscripts.md 對話記錄整理（2026-03-19）
