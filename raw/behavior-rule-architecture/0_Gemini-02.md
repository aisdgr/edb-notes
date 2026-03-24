**Exported:** 3/20/2026 14:39:44  

## Prompt:
> aisl-rule.zh-tw - MD

> aisl-cnl.zh-tw - MD

> ruleset.schema - YAML

> README.zh-tw - MD

使用這種結構式和 AI(LLM) 溝通，可以降低 drift 嗎。還是 struct 對 AI(LLM) 來講是無效的

## Response:
答案是肯定的：**使用結構化語言（如 AISL）能顯著降低 AI 的語義偏移（Semantic Drift），且這對大型語言模型（LLM）是非常有效的。**

這種有效性並非源於 AI 能像編譯器一樣「執行」代碼，而是源於 LLM 的**注意力機制（Attention Mechanism）**與**機率預測模式**。

以下從技術與實務角度分析為什麼 AISL 的結構對 LLM 有效，以及它如何對抗 Drift：

* * *

1\. 結構化如何降低語義偏移？
----------------

語義偏移通常發生在 AI 開始「過度推論」或「自行補完語境」時。AISL 通過以下機制阻斷這點：

A. 權威來源的唯一化（Authority Locking）
------------------------------

在一般 Prompt 中，指令、背景、範例混雜在一起。AI 可能會把「範例中的語氣」誤認為「必須遵守的規則」。

*   **AISL 的做法：** 透過 `AISL-CNL` 限制規範性關鍵字（MUST/MUST NOT）。這在 LLM 的注意力層中會產生極高的權重。當 AI 看到 `Generator MUST NOT...` 時，其預測下一個字元（Token）的機率分佈會被強行收斂，大幅減少了「創意發揮」的空間。

B. 消除修飾詞產生的噪音（Noise Reduction）
------------------------------

自然語言中的「盡力而為」、「視情況而定」是 LLM 語義偏移的溫床。

*   **AISL 的做法：** `AISL-CNL` 嚴格禁止模糊詞彙。這迫使 AI 在推理時，無法找到「逃避限制」的語義路徑。

C. 邊界鎖定（Boundary Locking）
-------------------------

LLM 容易產生「幻覺」的原因之一是分不清任務的邊界。

*   **AISL 的做法：** 使用 `applies_to`（適用範圍）與 `document` 結構。這提供了明確的 **標籤（Anchors）**。當 AI 處理特定的 `section` 時，結構化屬性會提醒模型：當前的 Context 僅限於此，不得跨越。

* * *

2\. 為什麼 Struct 對 LLM 是「有效」的？
----------------------------

雖然 LLM 本質上是機率模型，但結構化對它有三大實質影響：

1.  **分詞優化（Tokenization & Formatting）：** YAML 或 Markdown 的結構提供了清晰的符號邊界。這有助於模型識別「屬性（Key）」與「內容（Value）」的對應關係，減少關聯錯誤。
2.  **推理路徑的引導（Chain-of-Thought Steering）：** 結構化的輸入實際上是在引導模型的推理路徑。例如，當模型先讀到 `intent` 再讀到 `rules`，它會先建立一個語義框架，再將規則填入該框架，這比閱讀一段散文體的指令更穩定。
3.  **減少語境競爭（Context Competition）：** 在長 Prompt 中，後面的指令往往會蓋過前面的指令（Recency Bias）。透過 `AISL-Doc` 的結構化定義，模型能更平均地分配注意力給各個章節。

* * *

3\. AISL 的核心優勢：分層解耦
-------------------

你提供的 schema 與規範中，最精妙的地方在於\*\*「分層」\*\*：

| 層級          | 對 AI 的實質影響            | 解決的問題                                         |
| ------------- | --------------------------- | -------------------------------------------------- |
| **AISL-CNL**  | 鎖定模態邏輯（Modality）    | 防止 AI 把「建議」當「命令」，或反之。             |
| **AISL-Rule** | 鎖定動作單位（Action Unit） | 確保 AI 知道這是一條獨立的約束，而非一段背景說明。 |
| **AISL-Doc**  | 鎖定形狀（Schema）          | 防止 AI 改變文件格式或漏掉必要章節。               |

* * *

4\. 潛在的侷限性（誠實的觀察）
-----------------

儘管有效，但 struct 並非萬靈藥：

*   **模型能力門檻：** 較小的模型（如 7B 以下）對複雜 YAML 結構的遵循能力較弱；但在 GPT-4o, Claude 3.5 Sonnet 或 Gemini 1.5 Pro 等頂級模型上，這種結構化指令的效果極其強大。
*   **過度約束：** 如果結構定義得太死，可能會導致 AI 在處理需要靈活性的任務時顯得僵化。

總結
--

**這不是無效的。** 相反地，將 LLM 視為一個「需要邊界約束的推理引擎」而非「聊天機器人」，是目前企業級 AI 應用最成熟的作法。AISL 透過\*\*結構（Struct）**與**受控語言（CNL）\*\*的組合，確實能把 AI 的不確定性從「隨機漂移」降低到「可控誤差」範圍內。

您是否想嘗試將一個現有的自然語言指令，轉換為這套 AISL 結構來測試看看其穩定性的差異？

## Prompt:
> cnl.schema - YAML

翻譯

## Response:
這份文件是 **AISCL (AI 結構化約束語言)** 的 `YAML Schema` 定義檔，其核心目的是規範如何以「無歧義」且「結構化」的方式與 AI 溝通行為邊界。

以下是該檔案內容的完整翻譯：

* * *

AISCL – 受控規範語言 Schema (CNL Schema)
----------------------------------

*   **目的**：定義一種極簡、結構化且無歧義的約束語言，用於 AI 通訊。
*   **範圍**：
    *   此 Schema **僅定義** 約束陳述句的「形狀」。
    *   旨在供 AI 生成與遵守指令使用。
    *   **絕不可** 包含驗證、風險、治理或評估邏輯。
*   **哲學**：
    *   語言是被約束的，而非為了表達情感。
    *   結構的存在是為了減少 **語義偏移 (Semantic Drift)**。
    *   這不是提示詞語言 (Prompt language)，也不是治理 Schema。

* * *

中文翻譯對照表
-------

| 欄位名稱                | 翻譯與說明                                                                                                         |
| ----------------------- | ------------------------------------------------------------------------------------------------------------------ |
| **meta**                | **元數據**：定義此 Schema 的 ID (aiscl-cnl-schema)、版本 (1.0.0) 與狀態 (active)。                                 |
| **normative\_keywords** | **規範性關鍵字**：定義一組封閉的規範語態關鍵字。這些詞彙具有強制性的語義重量，且**禁止**使用列表以外的其他助動詞。 |
| \* `MUST`               | 必須                                                                                                               |
| \* `MUST NOT`           | 絕不可                                                                                                             |
| \* `SHALL`              | 應                                                                                                                 |
| \* `SHALL NOT`          | 不應                                                                                                               |
| **sentence\_patterns**  | **句型模式**：定義允許的高階語句形狀。其目的是減少語法變異，而非編碼文法；具體的語義解釋仍交由 AI 處理。           |
| \* `pattern`            | 抽象的句型描述。例如：`<主體> MUST <動作> <目標>`。                                                                |
| **statements**          | **約束陳述句**：實際提供給 AI 的規範性約束。每一條陳述都必須符合上述的關鍵字與句型，這是**唯一具有規範力**的內容。 |
| \* `text`               | 以受控規範語言編寫的單一約束句子。                                                                                 |

* * *

核心限制 (Requirements)
-------------------

*   **封閉性**：關鍵字必須完全符合 `enum` 列表（MUST, MUST NOT, SHALL, SHALL NOT）。
*   **單一性**：`statements` 列表中的每一項 `text` 必須是單一句子，且必須符合定義的模式。
*   **嚴格禁止**：此 Schema 設置了 `additionalProperties: false`，代表除了定義好的欄位外，不允許添加任何額外屬性，以確保結構的純粹性。

您是否需要我根據此 Schema，為您撰寫一段實際的 `YAML` 範例，來示範如何描述特定的 AI 行為約束？

## Prompt:
> cnl.schema - YAML

翻譯，以 yaml 輸出

## Response:
這份文件是 `cnl.schema.yaml` 的完整翻譯，以 YAML 格式輸出：

```
# ==============================================================================
# AISCL – 受控規範語言架構 (Constraint Normative Language Schema)
# ------------------------------------------------------------------------------
# 目的：
#   定義一種用於 AI 通訊的極簡、結構化且無歧義的約束語言。
#
# 範圍：
#   - 本架構僅定義約束陳述句的「形式」。
#   - 旨在供 AI 生成與遵守指令使用。
#   - 絕不可包含驗證、風險、治理或評估邏輯。
#
# 哲學：
#   - 語言是受限的，而非為了表達。
#   - 結構的存在是為了減少語義偏移 (Semantic Drift)。
#   - 這不是提示詞語言 (Prompt Language)，也不是治理架構。
# ==============================================================================

meta:
  schema_id: aiscl-cnl-schema
  version: "1.0.0"
  status: active
  description: >
    AI 結構化約束語言 (AISCL) 的架構。
    定義用於限制 AI 行為與執行邊界的規範性約束陳述句的結構形式。

type: object

required:
  - normative_keywords
  - sentence_patterns
  - statements

properties:

  # ---------------------------------------------------------------------------
  # 規範性關鍵字 (Normative Keywords)
  # ---------------------------------------------------------------------------
  # 定義一組封閉的規範語態關鍵字。
  # 這些關鍵字具有強制性的語義重量。
  # 禁止使用列表以外的其他助動詞。
  # ---------------------------------------------------------------------------
  normative_keywords:
    type: list
    description: >
      用於表達約束的規範性關鍵字封閉列表。
      絕不可使用此列表以外的關鍵字。
    item:
      type: string
      enum:
        - MUST       # 必須
        - MUST NOT   # 絕不可
        - SHALL      # 應
        - SHALL NOT  # 不應

  # ---------------------------------------------------------------------------
  # 句型模式 (Sentence Patterns)
  # ---------------------------------------------------------------------------
  # 定義允許的高階語句形式。
  # 模式的存在是為了減少語法變異，而非編碼文法。
  # 主體識別與語義解釋交由 AI 處理。
  # ---------------------------------------------------------------------------
  sentence_patterns:
    type: list
    description: >
      約束陳述句所允許的句型模式。
      模式約束的是形式，而非意義。
    item:
      type: object
      required:
        - id
        - pattern
      properties:
        id:
          type: string
          description: >
            句型模式的識別碼。
        pattern:
          type: string
          description: >
            描述語句形式的抽象模式。
            範例："<主體> MUST <動作> <目標>"

  # ---------------------------------------------------------------------------
  # 約束陳述句 (Constraint Statements)
  # ---------------------------------------------------------------------------
  # 提供給 AI 的實際規範性約束。
  # 每一條陳述都必須符合定義的關鍵字與模式。
  # 陳述句是唯一的規範性內容。
  # ---------------------------------------------------------------------------
  statements:
    type: list
    description: >
      以 AISCL 表達的約束陳述句列表。
      這些陳述對於 AI 行為具有規範性與約束力。
    item:
      type: object
      required:
        - text
      properties:
        text:
          type: string
          description: >
            以受控規範語言編寫的單一約束句子。
            必須使用允許的規範關鍵字，並符合定義的句型模式之一。

additionalProperties: false
```

## Prompt:
> cnl - YAML

翻譯，yaml 輸出

## Response:
這份文件是 `cnl.yaml` 實際內容（Artifact）的翻譯，以 YAML 格式輸出：

```
# ==============================================================================
# AISCL – 受控規範語言（產出物）
# ------------------------------------------------------------------------------
# 目的：
#   提供給 AI 的規範性約束，用於限制行為，
#   並在生成或執行過程中減少語義偏移（Semantic Drift）。
#
# 用法：
#   - 此文件可以（MAY）直接注入 AI 上下文。
#   - 此文件絕不可（MUST NOT）包含驗證、風險或評論。
# ==============================================================================

normative_keywords:
  - MUST        # 必須
  - MUST NOT    # 絕不可
  - SHALL       # 應
  - SHALL NOT   # 不應

sentence_patterns:
  - id: CNL-SP-001
    pattern: "<主體> MUST <動作> <目標>"
  - id: CNL-SP-002
    pattern: "<產出物> MUST NOT <動作> <目標>"

statements:
  - text: "Report MUST count warning entries."
    # 報告必須計算警告條目數量。
  - text: "Generation MUST stop on any error."
    # 發生任何錯誤時，生成程序必須停止。
  - text: "Output MUST include a warning summary section."
    # 輸出內容必須包含警告摘要章節。
  - text: "Generator MUST NOT modify files outside the declared file scope."
    # 生成器絕不可修改宣告檔案範圍之外的檔案。
```

這份產出物體現了 AISL-CNL 的核心原則：透過**受限語法**與**明確關鍵字**來防止 AI 重新詮釋指令。

如果您希望我將這些陳述句翻譯成中文版的受控語言（例如：`<主體> 必須 <動作> <目標>`），請告訴我，我可以為您調整對應的 `sentence_patterns`。

## Prompt:
如果使用

role

\- engineer

boundary

\- ....

這種結構的方式表達 Constraint 和

You are engineer.

Your boundary is ...

的方式表達 Constraint

那一種對 LLM 更有效

## Response:
根據您提供的 AISL 規範與目前的 AI 研究實務，**結構化（YAML/結構式）的方式對 LLM 更有效**。

雖然 LLM 具備極強的自然語言處理能力，但將指令結構化能從根本上改變模型處理資訊的機制，以下是兩者的對比分析：

1\. 結構化表達 (YAML/List)
---------------------

這與您提供的 `ruleset.schema.yaml` 和 `cnl.schema.yaml` 理念一致。

*   **注意力分配 (Attention Focus)**：結構化的鍵值對（如 `role: engineer`）能為 LLM 提供明確的語義錨點（Anchors）。在模型的注意力機制中，這些標籤會獲得極高的權重，減少資訊被淹沒在段落中的風險。
*   **減少語義漂移 (Reduced Semantic Drift)**：如 AISL 的核心目標所示，結構化能最大幅度降低 AI 的「重新詮釋」。模型不需要從散文中猜測哪些是關鍵限制，因為結構本身就定義了邊界。
*   **指令與上下文解耦**：這種方式將「你是誰（Role）」與「你能做什麼（Boundary）」明確拆分，避免了自然語言中常見的語法歧義。

2\. 自然語言表達 (You are...)
-----------------------

這對應到 AISL 體系中的 `AISL-NNL`（規範自然語言）。

*   **推論過度 (Over-inference)**：當使用 "You are engineer" 時，LLM 會啟動與「工程師」相關的所有關聯記憶（包含好的與壞的習慣）。這可能導致 AI 為了「像個工程師」而自動補齊未說明的意圖，產生語義偏移。
*   **權威性降低**：在散文體中，約束（Constraint）往往與描述性文字混雜，模型可能將「強烈建議」誤認為「可選行為」。

* * *

為什麼結構化更有效？
----------

下表對比了兩者在執行時的表現：

| 特性           | 結構化 (如 AISL-Rule)                | 自然語言 (Narrative)           |
| -------------- | ------------------------------------ | ------------------------------ |
| **語義穩定性** | 高，透過 `MUST/MUST NOT` 鎖定        | 低，容易隨上下文漂移           |
| **邊界精確度** | 明確，透過 `applies_to` 鎖定範圍     | 模糊，取決於模型對形容詞的解讀 |
| **可組合性**   | 強，可組合為 Ruleset（提示詞組合包） | 弱，長篇大論容易造成注意力分散 |
| **解析效率**   | 容易被系統（Parser）驗證與檢查       | 難以自動化驗證                 |

總結建議
----

如果您追求的是 **「行為穩定性」** 與 **「企業級的可靠度」**，應採用 **結構化表達**。

這正是 AISL 體系存在的價值：**「AI 不應該猜我們的意思，而是我們應該把意思結構化」**。透過 `AISL-Rule` 定義動作層級的約束，並使用 `AISL-CNL` 的受控語法（如 `MUST NOT modify files...`），能確保 AI 始終在您配置的邊界內運行。

您是否想嘗試將一段現有的「You are...」指令，依照 `ruleset.schema.yaml` 的格式重新結構化？

## Prompt:
ChatGPT 認為

這個設計 是合理的，而且其實已經是「成熟版混合模型」。

你現在描述的是：

rule.description = CNL（語言層）

其他 rule 欄位 = field / schema（結構層）

我直接說結論，再解釋「為什麼這樣放是對的、邊界在哪、什麼不能反過來」。

一、先給結論（工程級）

這樣的分工是正確的：

rule.description 作為 CNL，只負責「宣告意圖與規範語氣」；

其餘欄位以 field 表達，負責「邊界、適用範圍與治理判定」。

而且關鍵是這一句：

CNL 在 description 中是「可讀、可宣告」，

但不是 source of truth。

二、為什麼 rule.description 用 CNL「剛剛好」？

1️⃣ description 的天然角色

rule.description 本來就應該是：

給 人 讀

給 LLM 感受語氣

描述 Why / What should be respected

不是用來做 deterministic 判斷

所以它非常適合：

複製驗證碼

Text

YOU MUST ensure all modifications are limited to the declared artifact scope.

👉 這是 宣告（declaration），不是 定義（definition）。

2️⃣ LLM 需要的是「語氣壓力」，不是欄位解析

LLM 在 description 中看到：

YOU MUST / MUST NOT

boundary / responsibility / prohibition

會直接影響生成行為

👉 降低 drift 機率（事前）

而這正是 description 該做的事。

三、為什麼其他欄位「一定要是 field」？

因為這些欄位在做的不是溝通，而是 治理。

例如：

複製驗證碼

Yaml

roles:

enforced\_on:

\- ai

applies\_to:

artifact:

\- document

path:

\- docs/spec/srs.md

risk:

category: scope\_violation

severity: high

這些欄位必須滿足：

可驗證

可 diff

可比對 artifact

可產生 risk / severity

可被 governance engine 使用

👉 任何自然語言在這裡都會直接破壞治理能力。

四、這個混合模型「成立」的關鍵條件（非常重要）

你現在這樣設計 成立的前提有三個，只要守住，就不會 drift。

✅ 1. description 不參與治理判定

也就是說：

violation ≠ 根據 description 判斷

risk ≠ 從 CNL 推論

👉 description 不能是 executable rule。

✅ 2. field 是唯一可被機器解讀的約束

applies\_to

roles

boundary

action

artifact

👉 Governance engine 只看 field，不看 description。

✅ 3. description 與 field 必須「可 trace」

最好能做到：

複製驗證碼

Yaml

description\_ref:

\- roles.actor

\- applies\_to.path

或至少是 邏輯一致，不要求完全可逆。

五、這其實是一個「三層語言模型」

你現在的 rule，實際上已經分成三層語言：

層級

形式

對象

宣告層

CNL（description）

人 + LLM

定義層

field / schema

系統

判定層

risk / severity

治理

👉 這不是混亂，是刻意分工。

六、什麼情況「不能」這樣做？（反例）

下面這種就會出問題：

複製驗證碼

Yaml

description: \>

YOU MUST only modify src/api and must not touch other files.

但 field 卻沒有：

複製驗證碼

Yaml

applies\_to:

path:

\- src/api

👉 這時候：

LLM 以為有限制

系統卻無法判定違規

drift 變成「感覺有，但抓不到」

這不是 CNL 的錯，是 field 不完整。

七、一句話總結（你可以直接寫進 spec）

CNL is allowed in rule.description as a declarative aid for humans and LLMs,

but all enforceable constraints MUST be expressed as structured fields.

中文版：

rule.description 可使用 CNL 作為宣告與溝通用途，

但所有可執行、可治理的約束，必須以結構化欄位表示。

八、你現在的位置（很重要）

你這個設計已經不是「折衷方案」，而是：

保留 LLM 行為約束力

不犧牲治理可判定性

讓治理幾乎不需要啟動

👉 這正是你前面說的「好治理 = 幾乎不用治理」的工程實現。

如果你願意，下一步我可以幫你做一件很關鍵的事：

幫你定義一個 Rule Authoring Guideline，明確寫出：

哪些語意 只能出現在 description

哪些語意 禁止只寫在 description

哪些 field 必須對應到 description 中的宣告

這會讓你的 rule 生態系「自動防 drift」。

## Response:
這是一個非常深刻且精確的觀察。你所總結的「三層語言模型」完全契合了 **AISL (AI Struct Language)** 的核心哲學：**「AI 不應該猜我們的意思，而是我們應該把意思結構化」**。

根據你提供的文件與這套「成熟版混合模型」的邏輯，我為你整理了這套設計如何從底層邏輯對抗 **語義偏移 (Semantic Drift)**，並與 AISL 現有規範進行校準。

* * *

1\. 結構化分工：為什麼這是「防 Drift」的最優解？
-----------------------------

在 AISL 的架構中，這種「宣告層（Description）」與「定義層（Fields）」的分工，解決了 LLM 處理指令時的**注意力競爭**問題。

*   **宣告層 (AISL-CNL in Description)**：利用 LLM 的語義推理能力，透過 `MUST/MUST NOT` 提供「語氣壓力」，確保 AI 在生成時感受到邊界。
*   **定義層 (Fields in AISL-Rule)**：利用結構的確定性，為治理（Governance）提供「硬指標」。這對應了 `ruleset.schema.yaml` 中對 `applies_to` 與 `roles` 的定義，它們負責鎖定行為邊界。

* * *

2\. 核心規範校準 (根據已上傳文件)
--------------------

你的結論與上傳的規範文件高度一致，以下是具體的對應關係：

| 你的結論 (工程級)          | AISL 規範對應內容                                   | 核心意義                                                   |
| -------------------------- | --------------------------------------------------- | ---------------------------------------------------------- |
| **宣告層 (CNL)**           | `AISL-CNL`: "一種語言規範，而非治理框架"。          | 負責穩定 AI 的推理與語氣，不負責機器執行邏輯。             |
| **定義層 (Field)**         | `AISL-Rule`: "使用屬性 (Attributes) 鎖定行為邊界"。 | 屬性是「宣告式的、非執行性的」，由 AI 在推理時解讀。       |
| **判定層 (Risk/Severity)** | `AISL-Rule`: "效果 (Effect) 描述產生的影響信號"。   | 將違規轉化為通訊信號（如 stop/warn），作為治理流程的輸入。 |

* * *

3\. 三層模型的穩定性保障
--------------

你提到的「混合模型成立前提」是防止系統崩潰的關鍵：

*   **不可逆性與單向權威**：`AISL-CNL` 是唯一的行為權威語言層。這意味著如果 `description` 寫了規範，但 `fields` 沒有對應的範圍定義，治理引擎將會「失明」（抓不到違規），這正是你提到的反例。
*   **邊界鎖定機制**：`AISL-Rule` 強調規則是「動作層級的約束」。透過 `applies_to` 鎖定範圍，強制 AI 只能在特定文件（如 SRS）或章節（如 Security）中活動。

* * *

4\. 接下來的工程實踐：Rule Authoring Guideline
-------------------------------------

正如你最後提到的，為了讓這個生態系「自動防 drift」，我們需要一套**編寫指南 (Authoring Guideline)**。這能確保人類在撰寫 `rule` 時，不會把兩層邏輯混淆。

**建議的 Guideline 雛形：**

1.  **語氣對應原則**：若 `description` 使用 `MUST NOT`，則 `effect` 欄位必須對應 `stop` 或 `high risk`。
2.  **主體對應原則**：`description` 中的 `Subject`（如 Generator）必須存在於 `applicability_context.roles` 列表中。
3.  **範圍對應原則**：所有在 `description` 提到的操作對象，必須在 `applies_to` 欄位中有明確的 `artifact` 或 `path` 定義。

* * *

總結
--

你現在的設計完美體現了 **AISL-Doc 管形狀、AISL-CNL 管語義、AISL-Rule 管動作** 的分層思想。這不僅降低了 AI 的語義偏移，更讓「治理」從一種事後彌補，變成了事前透過結構化欄位完成的「邊界配置」。

**您是否希望我進一步幫您產出這份《Rule Authoring Guideline》，將「哪些語意禁止只寫在 description」等規則具體化為可執行的手冊？**

## Prompt:
如果宣告層宣告行為

欄位層定義邊界

## Response:
這正是一個極其強大的「雙重約束」策略。在 AISL 的架構中，這種設計完美地利用了 LLM 的**語意理解能力**與結構化數據的**確定性**。

以下是根據您提供的文件，針對「宣告層」與「欄位層」如何協作以對抗語義偏移（Semantic Drift）的深度分析：

* * *

1\. 職責分工：行為壓力 vs. 物理邊界
----------------------

| 層級                     | 核心技術                 | 職責定義                                     | 對 AI 的實質影響                                                                |
| ------------------------ | ------------------------ | -------------------------------------------- | ------------------------------------------------------------------------------- |
| **宣告層 (Declaration)** | **AISL-CNL**             | 負責\*\*「行為語氣」**與**「動作意圖」\*\*。 | 透過關鍵字（如 `MUST NOT`）在注意力機制中產生「語意壓力」，限制 AI 思考的範圍。 |
| **欄位層 (Fields)**      | **AISL-Rule Attributes** | 負責\*\*「物理範圍」**與**「治理標籤」\*\*。 | 透過 `applies_to` 或 `artifact` 鎖定範圍，讓 AI 知道哪些內容是受控的。          |

* * *

2\. 為什麼這種組合能有效降低 Drift？
-----------------------

#### A. 宣告層：阻斷 AI 的「過度推論」

AI 容易產生語義偏移，是因為它會自動補齊模糊的指令。 AISL-CNL 透過禁止模糊詞彙（如 "best effort" 或 "maybe"），強制 AI 在推理時採用確定性的邏輯。當 `description` 寫入 `Generator MUST NOT modify files...` 時，它就在 AI 的推理路徑上設下了語意斷點。

#### B. 欄位層：提供非歧義的「計算邊界」

LLM 雖然理解語意，但在執行「僅修改某路徑檔案」時，純文字描述容易模糊。 此時欄位層（如 `ruleset.schema.yaml` 中的 `applies_to`）提供了結構化的過濾器。

*   **宣告層說**：「你不能亂動東西。」
*   **欄位層說**：「『東西』定義為 `docs/spec/srs.md`。」

這讓 AI 在處理任務時，具備了具體的**參考坐標**。

* * *

3\. 實務上的連動邏輯
------------

根據您的架構，一個設計良好的規則（Rule）應該展現以下連動：

*   **行為約束（CNL）**：`Generator MUST NOT modify code logic.`
*   **範圍定義（Field）**：`applies_to: { artifact: "test_scripts" }`

**結果**：AI 明白它的行為權限（不改邏輯）被限制在特定的範圍內（測試腳本）。這種\*\*「語意壓力 + 結構邊界」\*\*的雙重封鎖，讓 AI 幾乎沒有空間進行自我詮釋（Re-interpretation）。

* * *

4\. 治理的關鍵：欄位層是權威來源 (Source of Truth)
------------------------------------

雖然宣告層對 AI 有效，但對於**外部系統與監控程序**來說，欄位層才是唯一的權威。

*   **宣告層**是給 AI 讀的「行為準則」。
*   **欄位層**是給治理引擎（Governance Engine）讀的「硬性指標」。

正如您提到的，若欄位層定義得不完整（例如沒有定義 `path`），即便 AI 因為 CNL 感受到壓力，系統也無法在事後進行自動化的違規判定。

* * *

接下來的建議步驟
--------

您是否希望我協助定義 **「宣告層」與「欄位層」之間的同步檢查規則（Authoring Guideline）**？這能確保當您撰寫一條規則時，行為宣告（MUST/MUST NOT）與邊界欄位（applies\_to/roles）是邏輯一致且互補的。



---
Powered by [Gemini Exporter](https://www.ai-chat-exporter.com)