# CONCEPT 結構調整計劃

**Generated:** 2026-03-27
**Source:** task/concept.md
**Status:** Draft

---

## 一、分析摘要

### 1.1 現況

`task/concept.md` 是一份詳細的對話記錄，包含如何從 NOTES 系統化整理成有敘事結構的 CONCEPT 的完整討論。

**核心問題診斷：**
- ✅ 已定義三層次體系：NOTES → OUTLINE → CONCEPT
- ✅ 已提出完整的 10 章節結構
- ❌ 文件仍是原始對話格式，尚未整理
- ❌ 缺少明確的 Boundary 定義
- ❌ 敘事主線不明確
- ❌ 衝突沒有顯性化

### 1.2 核心概念

**三層次定位：**

| 層次        | 定位             | 特性                             |
| ----------- | ---------------- | -------------------------------- |
| **NOTES**   | 去噪後的原始觀察 | 可矛盾、可重複、不完整           |
| **OUTLINE** | 語義聚集層       | 依主題分桶，不建立敘事，不解衝突 |
| **CONCEPT** | 有邊界的概念系統 | 定義邊界、建立敘事、顯性化衝突   |

**CONCEPT 的本質：**
- 不是「整理結果」
- 而是 **Boundary Decision + Narrative Construction**

---

## 二、核心問題

### 2.1 缺少明確 Boundary

**問題：** 當前 BRA (Behavior Rule Architecture) 文件沒有明確聲明：
- ✓ 什麼是邊界內 (In-Scope)
- ✓ 什麼是邊界外 (Out-of-Scope)

**影響：**
- 混淆 BRA 與 BCM、Runtime 的關係
- 導致後續 Paper 無法清晰引用
- 造成語義污染

### 2.2 敘事主線不明確

**問題：** 有內容但沒有建立推進路徑：
```
問題 → 為何不行 → 解法 → 為何成立
```

**影響：**
- 難以被讀者理解
- 無法轉換為發表內容
- 失去論證價值

### 2.3 衝突沒有顯性化

**問題：** 以下關鍵衝突分散在不同段落，未被明確標記：
- Policy vs Constraint
- Attribute vs Imperative
- Boundary inside vs outside
- Engineering View vs Governance View

**影響：**
- 隱藏了最有價值的貢獻點
- 無法直接轉換為論文 contribution

---

## 三、調整策略

### 3.1 立即行動 (Phase 1)

#### 3.1.1 建立 CONCEPT 模板文件

**目標：** 創建標準化的 CONCEPT 結構模板

**行動：**
```
創建文件：.claude/skills/content-pipeline/templates/concept.md
```

**內容結構：**
```markdown
# [Concept Title]

## Metadata
- Version:
- Date:
- Status: Draft / Evolving / Stabilized
- Scope:
- Source:

## 1. Concept Boundary
### In-Scope
### Out-of-Scope

## 2. Problem Framing
## 3. Core Narrative
### 3.1 Observed Phenomena
### 3.2 Emerging Patterns
### 3.3 Structural Tensions
### 3.4 Directional Hypotheses

## 4. Conceptual Structure
## 5. Classified Statements
### 5.1 Established Conclusions [C-XX]
### 5.2 Open Questions [Q-XX]
### 5.3 Conflicts / Tensions [T-XX]
### 5.4 Undefined / Ambiguous Zones [U-XX]

## 6. Implications
### 6.1 Engineering Implications
### 6.2 Governance Implications
### 6.3 Research Implications

## 7. Extraction Map
→ Paper Candidates
→ Medium Topics
→ Tool / Product

## 8. Out-of-Scope but Relevant Notes [O-XX]
### 8.1 Adjacent Concepts
### 8.2 Cross-Domain Signals
### 8.3 Early Fragments

## 9. Evolution Backlog
### 9.1 TODO (Concept 完整性)
### 9.2 FEATURE (延伸能力)
### 9.3 RESOURCING (資源/依賴)

## 10. Evolution Log
```

#### 3.1.2 制定 CONCEPT 轉換規則

**目標：** 在 `rules/` 目錄建立 `concept.yaml`

**關鍵規則：**
```yaml
stage: CONCEPT
input: OUTLINE
output: CONCEPT

rules:
  - MUST define Concept Boundary first
  - MUST preserve conflicts, not resolve them
  - MUST establish narrative continuity
  - MUST classify statements by status
  - MUST separate in-scope from out-of-scope
  - MUST NOT prematurely converge
  - MUST NOT drop technical content

workflow:
  1. Read OUTLINE files
  2. Define Boundary (In-Scope / Out-of-Scope)
  3. Construct Core Narrative
     - Problem → Tension → Mechanism → Implication
  4. Build Conceptual Structure
  5. Classify Statements
     - Conclusions [C]
     - Questions [Q]
     - Conflicts [T]
     - Undefined [U]
  6. Extract Implications
  7. Create Extraction Map
  8. Collect Out-of-Scope Notes [O]
  9. Build Evolution Backlog
  10. Generate Evolution Log
```

### 3.2 短期調整 (Phase 2)

#### 3.2.1 重構現有 BRA 文件

**目標：** 將 `behavior-rule-architecture.md` 轉換為符合 CONCEPT 結構

**步驟：**
1. 明確定義 BRA 的 Boundary
   - In-Scope：NNL, RNL, Rule, Ruleset, Execution
   - Out-of-Scope：Boundary definition, BCM, Runtime governance

2. 建立敘事主線
   - Problem：AI behavior 無約束導致不可預測
   - Tension：Policy vs Constraint, Declarative vs Imperative
   - Mechanism：BRA (NNL → RNL → Rule → Ruleset)
   - Implication：可治理的 AI 產碼

3. 顯性化衝突
   ```
   [T-01] Policy vs Constraint
   - Policy View：寬鬆、可覆蓋
   - Constraint View：嚴格、不可違反
   - Conflict：治理彈性 vs 工程安全
   ```

4. 重組內容到 10 個章節

#### 3.2.2 驗證 OUTLINE 品質

**目標：** 確保 OUTLINE 層只做「聚集」，不做「整合」

**檢查點：**
- ❌ OUTLINE 是否做過早的因果推論？
- ❌ OUTLINE 是否嘗試解決衝突？
- ❌ OUTLINE 是否建���了不必要的主線？

**修正原則：**
- OUTLINE 應該只是分桶收納
- 允許重複、允許矛盾
- 不追求完整性

### 3.3 中期優化 (Phase 3)

#### 3.3.1 建立 Pipeline 自動化

**目標：** 將 `/concept` 指令整合到 content-pipeline

**實現：**
```yaml
# .claude/skills/content-pipeline/stages/concept.yaml
mode: CONCEPT
trigger: /concept

execution:
  1. Load OUTLINE files (from previous stage)
  2. Apply rules/concept.yaml
  3. Generate output using templates/concept.md
  4. Create report in reports/concept-YYYYMMDD-NN.md
```

#### 3.3.2 建立品質檢查

**目標：** 確保 CONCEPT 符合標準

**檢查項：**
- [ ] Boundary 是否明確定義？
- [ ] 敘事是否連貫？
- [ ] 衝突是否顯性化？
- [ ] 分類是否完整 (C/Q/T/U)？
- [ ] Out-of-Scope 是否清晰分離？
- [ ] Evolution Backlog 是否可執行？

---

## 四、執行路徑

### 4.1 優先級排序

| 優先級 | 任務                   | 原因                             |
| ------ | ---------------------- | -------------------------------- |
| **P0** | 建立 CONCEPT 模板      | 基礎設施，所有後續工作依賴此文件 |
| **P0** | 制定 concept.yaml 規則 | 定義轉換邏輯，確保一致性         |
| **P1** | 重構現有 BRA           | 驗證模板可行性，產生第一個範例   |
| **P1** | 驗證 OUTLINE 品質      | 避免上游污染下游                 |
| **P2** | Pipeline 自動化        | 提升效率，但非立即必要           |
| **P2** | 品質檢查               | 長期維護所需                     |

### 4.2 時間估算

| 階段           | 工作量   | 建議時間 |
| -------------- | -------- | -------- |
| Phase 1 (立即) | 2-3 小時 | Day 1    |
| Phase 2 (短期) | 4-6 小時 | Day 2-3  |
| Phase 3 (中期) | 6-8 小時 | Week 1-2 |

### 4.3 成功標準

**Phase 1 完成：**
- ✅ `templates/concept.md` 創建
- ✅ `rules/concept.yaml` 創建
- ✅ 可透過 `/concept` 指令調用

**Phase 2 完成：**
- ✅ `behavior-rule-architecture.md` 重構完成
- ✅ Boundary 明確定義
- ✅ 至少 3 個衝突被顯性化
- ✅ Classification 區完整 (C/Q/T/U)

**Phase 3 完成：**
- ✅ Pipeline 支持自動轉換
- ✅ 品質檢查通過率 > 90%

---

## 五、風險與緩解

### 5.1 風險識別

| 風險                              | 影響 | 機率 | 緩解措施                           |
| --------------------------------- | ---- | ---- | ---------------------------------- |
| 模板過於複雜導致難以使用          | 高   | 中   | 提供簡化版本，先驗證核心結構       |
| OUTLINE 品質不佳導致 CONCEPT 污染 | 高   | 中   | 建立上游品質檢查                   |
| 過早收斂，遺失有價值的衝突        | 高   | 高   | 強調「保留衝突」原則，在規則中明確 |
| Boundary 定義不清                 | 高   | 高   | 要求每個 CONCEPT 必須先寫 Boundary |

### 5.2 關鍵決策

**決策 1：CONCEPT 是否包含範例？**
- 建議：**不包含**
- 理由：CONCEPT 是概念層，範例應放在 Paper/Post

**決策 2：Classification 是否需要 Confidence Level？**
- 建議：**可選**
- 理由：初期簡單為主，穩定後再加

**決策 3：Out-of-Scope 章節是否需要單獨文件？**
- 建議：**不單獨**
- 理由：保持在同一文件，方便追蹤邊界變化

---

## 六、下一步行動

### 6.1 立即執行 (Today)

- [ ] 創建 `templates/concept.md`
- [ ] 創建 `rules/concept.yaml`
- [ ] 在 CLAUDE.md 中添加 `/concept` 指令說明

### 6.2 本週執行

- [ ] 重構 `behavior-rule-architecture.md`
- [ ] 驗證至少 2 份 OUTLINE 品質
- [ ] 編寫 CONCEPT 轉換範例報告

### 6.3 持續改進

- [ ] 收集實際使用反饋
- [ ] 優化模板和規則
- [ ] 建立 CONCEPT 品質評分機制

---

## 七、參考資料

### 7.1 相關文件

- `.claude/CLAUDE.md` - 系統主控文件
- `task/concept.md` - 原始對話記錄
- `behavior-rule-architecture.md` - 待重構的目標文件

### 7.2 關鍵原則回顧

1. **Boundary First：** CONCEPT 必須先定義邊界
2. **Narrative Over Classification：** 敘事優先於分類
3. **Preserve Conflicts：** 衝突是核心資產
4. **No Premature Convergence：** 允許矛盾和未定義

### 7.3 成功模式

```
好的 CONCEPT = 清晰的 Boundary + 連貫的敘事 + 顯性化的衝突 + 可執行的 Backlog
```

---

**End of Plan**
