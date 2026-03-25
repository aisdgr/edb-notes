# AISL 結構化語言與語義漂移控制：Gemini 對話筆記

**來源：** Gemini 對話（2026/3/20）
**主題：** 結構化語言對 LLM 的有效性、AISL 設計哲學、CNL Schema、三層語言模型

---

## 結構化語言對 LLM 的有效性

### 核心結論

使用結構化語言（如 AISL）能顯著降低 AI 的語義偏移（Semantic Drift），對大型語言模型（LLM）非常有效。

有效性並非源於 AI 能像編譯器一樣「執行」代碼，而是源於 LLM 的**注意力機制（Attention Mechanism）**與**機率預測模式**。

### 降低語義偏移的三個機制

1. **權威來源唯一化（Authority Locking）**
   - 一般 Prompt 中指令、背景、範例混雜，AI 可能將範例語氣誤認為規則
   - AISL 透過 CNL 限制規範性關鍵字（MUST/MUST NOT），在注意力層產生極高權重，強行收斂 Token 機率分佈

2. **消除修飾詞噪音（Noise Reduction）**
   - 自然語言中的「盡力而為」、「視情況而定」是語義偏移溫床
   - AISL-CNL 嚴格禁止模糊詞彙，迫使 AI 無法找到「逃避限制」的語義路徑

3. **邊界鎖定（Boundary Locking）**
   - LLM 產生幻覺的原因之一是分不清任務邊界
   - `applies_to` 與 `document` 結構提供明確標籤（Anchors），限制 Context 範圍

### 結構化對 LLM 的三大實質影響

1. **分詞優化（Tokenization & Formatting）**：YAML/Markdown 提供清晰符號邊界，減少關聯錯誤
2. **推理路徑引導（Chain-of-Thought Steering）**：結構化輸入引導模型推理路徑，先建語義框架再填規則
3. **減少語境競爭（Context Competition）**：長 Prompt 中後面的指令蓋過前面的指令（Recency Bias），結構化定義讓模型平均分配注意力

### 潛在侷限性

- **模型能力門檻**：較小模型（7B 以下）對複雜 YAML 結構遵循能力較弱；頂級模型（GPT-4o、Claude 3.5 Sonnet、Gemini 1.5 Pro）效果極強
- **過度約束**：結構定義太死可能導致 AI 在需要靈活性的任務上顯得僵化

---

## 結構化 vs 自然語言表達 Constraint

### 對比結論

結構化（YAML/結構式）對 LLM 更有效。

| 特性 | 結構化（如 AISL-Rule） | 自然語言（Narrative） |
|------|------------------------|----------------------|
| 語義穩定性 | 高，透過 MUST/MUST NOT 鎖定 | 低，容易隨上下文漂移 |
| 邊界精確度 | 明確，透過 applies_to 鎖定範圍 | 模糊，取決於模型對形容詞的解讀 |
| 可組合性 | 強，可組合為 Ruleset | 弱，長篇大論造成注意力分散 |
| 解析效率 | 可被 Parser 驗證與檢查 | 難以自動化驗證 |

### 自然語言的問題

- **推論過度（Over-inference）**："You are engineer" 啟動所有工程師相關關聯記憶（含壞習慣），AI 自動補齊未說明的意圖
- **權威性降低**：約束與描述性文字混雜，模型可能將「強烈建議」誤認為「可選行為」

### 結構化的核心邏輯

- 結構化將「你是誰（Role）」與「你能做什麼（Boundary）」明確拆分
- **「AI 不應該猜我們的意思，而是我們應該把意思結構化」**

---

## AISL-CN Schema

### Schema 結構

CNL Schema 定義受控規範語言的結構形式：

| 欄位 | 用途 |
|------|------|
| `meta` | Schema ID、版本、狀態 |
| `normative_keywords` | 封閉的規範語態關鍵字（MUST / MUST NOT / SHALL / SHALL NOT） |
| `sentence_patterns` | 允許的句型模式（減少語法變異，非編碼文法） |
| `statements` | 實際的規範性約束陳述句（唯一具有規範力的內容） |

### 核心限制

- **封閉性**：關鍵字必須完全符合 enum 列表
- **單一性**：每條 text 必須是單一句子，符合定義的模式
- **嚴格禁止**：`additionalProperties: false`，確保結構純粹性

### AISL 設計哲學

- 語言是被約束的，非為了表達情感
- 結構的存在是為了減少 Semantic Drift
- 不是 Prompt Language，也不是治理 Schema

---

## 三層語言模型

### 來源

此概念來自 ChatGPT 的分析，Gemini 進行驗證與校準。

### 三層結構

| 層級 | 形式 | 對象 | 職責 |
|------|------|------|------|
| 宣告層 | CNL（description） | 人 + LLM | 宣告行為語氣與動作意圖 |
| 定義層 | field / schema | 系統 | 定義物理範圍與治理標籤 |
| 判定層 | risk / severity | 治理 | 違規轉化為通訊信號 |

### 與 AISL 的對應

| 工程結論 | AISL 規範對應 | 核心意義 |
|----------|--------------|---------|
| 宣告層（CNL） | AISL-CNL：語言規範而非治理框架 | 穩定 AI 推理與語氣，不負責機器執行 |
| 定義層（Field） | AISL-Rule：Attributes 鎖定行為邊界 | 宣告式、非執行性，由 AI 推理解讀 |
| 判定層（Risk/Severity） | AISL-Rule：Effect 描述影響信號 | 違規轉化為治理流程輸入 |

### 成立的關鍵條件

1. **description 不參與治理判定**：violation ≠ 根據 description 判斷，description 不能是 executable rule
2. **field 是唯一可被機器解讀的約束**：Governance engine 只看 field，不看 description
3. **description 與 field 必須可 trace**：邏輯一致，不要求完全可逆

### 反例：何時會出問題

```yaml
description: >
  YOU MUST only modify src/api and must not touch other files.
# 但 field 中沒有：
# applies_to:
#   path:
#     - src/api
```

結果：LLM 以為有限制，系統卻無法判定違規。這不是 CNL 的錯，是 field 不完整。

### 核心規範

> CNL is allowed in rule.description as a declarative aid for humans and LLMs, but all enforceable constraints MUST be expressed as structured fields.

中文：
> rule.description 可使用 CNL 作為宣告與溝通用途，但所有可執行、可治理的約束，必須以結構化欄位表示。

---

## 宣告層與欄位層的雙重約束

### 職責分工

| 層級 | 核心技術 | 職責 | 對 AI 的實質影響 |
|------|----------|------|----------------|
| 宣告層 | AISL-CNL | 行為語氣 + 動作意圖 | 關鍵字在注意力機制產生語義壓力 |
| 欄位層 | AISL-Rule Attributes | 物理範圍 + 治理標籤 | applies_to/artifact 鎖定受控範圍 |

### 協作邏輯

- 宣告層說：「你不能亂動東西。」
- 欄位層說：「『東西』定義為 `docs/spec/srs.md`。」
- 結果：**語義壓力 + 結構邊界的雙重封鎖**，AI 幾乎沒有空間進行自我詮釋

### 權威來源

- 宣告層：給 AI 讀的「行為準則」
- 欄位層：給治理引擎讀的「硬性指標」（Source of Truth）

---

## Rule Authoring Guideline 建議

### 語義對應原則

1. **語氣對應原則**：若 description 使用 MUST NOT，則 effect 欄位必須對應 stop 或 high risk
2. **主體對應原則**：description 中的 Subject 必須存在於 `applicability_context.roles` 列表中
3. **範圍對應原則**：description 中提到的操作對象必須在 applies_to 欄位中有明確的 artifact 或 path 定義

---

## 編輯檢查

- [x] 依原始對話時間排序
- [x] 保留所有有意義內容
- [x] 只去除噪音，非刪除語意
- [x] 保留推論過程與被推翻的推論
- [x] 將對話痕跡改寫為可閱讀的筆記結構
- [x] 避免重複敘述同一內容
