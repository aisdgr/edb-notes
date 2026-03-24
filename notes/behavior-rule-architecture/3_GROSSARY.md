# NOTES.md 術語調整說明

**調整日期**：2026-03-19
**調整目的**：將早期設計的 "Constraint" 用語調整為現在的設計（Rule → Policy / Constraint）

---

## 調整總覽

### 早期設計（RuleSpecification.md, RulesetSpecification.md）
- 使用 **Constraint** 作為頂層概念
- **未區分** MUST 和 MUST NOT 的語義差異
- 統稱為 "constraint" 或 "約束"

### 實際實作發現

| 維度              | MUST（政策）                              | MUST NOT（約束）                          |
| ----------------- | ----------------------------------------- | ----------------------------------------- |
| **類型**          | **Policy**                                | **Constraint**                            |
| **觸發可觀測性**  | ❌ **無法觀測**                            | ✅ **可以觀測**                            |
| **Evidence 生成** | ❌ **不會留下 Evidence**                   | ✅ **可以留下 Evidence**                   |
| **對 AI 的意義**  | **期望**（期望性行為）                    | **限制**（限制性行為）                    |
| **強制性等級**    | **中等**                                  | **高**                                    |
| **執行時機**      | AI 應該主動執行                           | AI 絕對不可執行                           |
| **稽核方式**      | 檢查「是否做到」（positive verification） | 檢查「是否違反」（negative verification） |

### 當前設計（concept.md）

**架構調整**：
```
Rule（頂層概念）
├── Policy（MUST）- 期望性行為，強制性中等
└── Constraint（MUST NOT）- 限制性行為，強制性高
```

---

## NOTES.md 中的調整項目

### 1. ✅ 核心金句調整（第 1.10 節）

**行 336-338**
```diff
- > **Constraints fail not because models refuse them, but because models reinterpret them.**
+ > **Rules fail not because models refuse them, but because models reinterpret them.**
+ >
+ > （早期設計原文：Constraints fail not because models refuse them, but because models reinterpret them.）
```

**行 341-343**
```diff
- > **Ruleset defines a decision constraint lens, not a set of executable behaviors.**
+ > **Ruleset defines a decision rule lens, not a set of executable behaviors.**
+ >
+ > （早期設計原文：Ruleset defines a decision constraint lens, not a set of executable behaviors.）
```

**調整理由**：保留早期設計原文作為歷史記錄，但更新為現在的設計用語。

---

### 2. ✅ RNL 規範標題調整（第 1.12 節）

**行 382-387**
```diff
- ### Constraint 的 Rule Normative Language (RNL) 規範
+ ### Rule 規範性核心的 Rule Normative Language (RNL) 規範
+
+ > **注意**：在 RuleSpecification.md 早期設計中，使用 `constraint` 欄位表示規範性核心，但實際上該欄位包含 **Policy（MUST）** 和 **Constraint（MUST NOT）** 兩種類型。現在的設計將這兩種類型明確區分。

**RNL 的必要性**
- - MUST 使用 Rule Normative Language (RNL)
+ - Policy 或 Constraint MUST 使用 Rule Normative Language (RNL)
```

**調整理由**：明確說明 RNL 規範適用於 Rule 的規範性核心（包括 Policy 和 Constraint）。

---

### 3. ✅ 規範獨占性調整（第 1.12 節）

**行 399-414**
```diff
- ### Constraint 的規範獨占性（詳細說明）
+ ### Rule 規範性核心的獨占性（詳細說明）

**核心原則**
- > **Constraint defines behavior.
- > Everything else explains context.**
+ > **Policy 或 Constraint 定義行為。
+ > 其餘一切皆在解釋背景。**
+ >
+ > （早期設計原文：Constraint defines behavior. Everything else explains context.）

**其他欄位的限制**
不得：
- 強加強制性行為
- - 覆寫或削弱 constraint
+ - 覆寫或削弱 Policy 或 Constraint
- 引入替代解釋
```

**調整理由**：更新為現在的設計用語，但保留早期設計原文作為歷史記錄。

---

### 4. ✅ Boundary 說明調整（第 1.11 節）

**行 528**
```diff
- #### `boundary` — WHERE the constraint applies
- - Declares the **decision scope** in which the constraint is relevant
+ #### `boundary` — WHERE the rule applies
+ - Declares the **decision scope** in which the rule (Policy or Constraint) is relevant
```

**調整理由**：Boundary 適用於整個 Rule，而非僅限於 Constraint。

---

### 5. ✅ Risk Trigger 決策說明（第 1.13 節）

**行 753**
```diff
- - BRA = static constraint definition（靜態約束定義）
+ - BRA = static rule definition（靜態規則定義）
```

**調整理由**：BRA 定義的是 Rule（包括 Policy 和 Constraint），而非僅限於 Constraint。

---

### 6. ✅ 架構層級說明調整（第 1.13 節，第 2.3 節）

**行 963**
```diff
Rule Architecture Layer (atomic constraint)
+ Rule Architecture Layer (atomic rule)
```

**行 1095**
```diff
Rule Architecture Layer (atomic constraint)
+ Rule Architecture Layer (atomic rule)
```

**調整理由**：Rule 是原子單元，Constraint 只是 Rule 的一種類型。

---

## 保留原樣的部分

以下部分保留原樣，因為它��是歷史性的範例或技術描述：

### 1. YAML 範例中的 `constraint` 欄位
```yaml
# 行 162, 515 - 保留原樣
constraint: >
  AI 絕不可 (MUST NOT) 在缺少必要輸入時繼續生成。
```
**理由**：這是 RuleSpecification.md 早期設計的實際 schema，保留作為歷史記錄。

### 2. 術語對照表（第 0 節）
已經正確記錄了早期用語和當前用語的對照，無需調整。

### 3. "constraint graph system"（行 771）
保留原樣，因為這是 Rule Relations 的技術描述。

---

## 術語對照表（總結）

| 早期用語（本文件中） | 當前用語（concept.md） | 說明                                |
| -------------------- | ---------------------- | ----------------------------------- |
| Constraint（頂層）   | Rule（頂層）           | 頂層概念                            |
| constraint（小寫）   | Policy / Constraint    | 根據 MUST / MUST NOT 區分           |
| 約束                 | 規則（Rule）           | 頂層概念                            |
| 單一約束             | Policy 或 Constraint   | 視 MUST / MUST NOT 而定             |
| 約束獨占性           | 規範性核心             | Policy 和 Constraint 都是規範性核心 |
| atomic constraint    | atomic rule            | Rule 是原子單元                     |

---

## 論文寫作注意事項

1. **統一使用 Rule** 作為頂層概念
2. **明確區分 Policy 和 Constraint**，並說明其差異
3. **強調觀測性和強制性差異**：這是實務上的重要發現
4. **使用正確術語**：
   - ✅ "Rule 分為 Policy 和 Constraint 兩種類型"
   - ✅ "Rule 的規範性核心（Policy 或 Constraint）"
   - ❌ "Constraint 包含 MUST 和 MUST NOT"
   - ❌ "Constraint 定義行為"（應該說 "Rule 的規範性核心定義行為"）

---

**最後更新**：2026-03-19
