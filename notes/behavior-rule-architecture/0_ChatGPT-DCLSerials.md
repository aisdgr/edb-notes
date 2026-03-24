# DCL Serials: AI 工程風險、觀點文章系列與產品策略

## AI 對工程師的就業衝擊

### 主要報導內容

美國程式設計師就業率下降約 27.5%，主要衝擊集中於初級職位與 22-25 歲年輕開發者，基礎性編碼任務可被 AI 工具取代。

### 背後的背景與事實脈絡

- 程式設計 vs 軟體開發：就業下降主要集中在「程式設計員」等基礎編碼工作，「軟體開發者」或更高階工程師的需求仍相對穩定或成長
- AI 正在取代「例行性、可標準化」的工作任務，這是長期趨勢的一部分

### 專家觀點

- AI 可以取代重複性編碼，但尚未能完全取代複雜的系統設計、維護與協作工作
- AI 會改變對技能的需求，提高人類工作的附加價值部分（創造性解決問題、溝通與戰略思維）

---

## 對產品、方法論與投資者的影響分析

### 對產品的影響

- 文章不是行銷，是「使用者預訓練」——先把錯誤使用方式從使用者腦中刪掉
- 讓產品「不需要討好初階使用者」，自然篩選只想要效率的人、只想要 prompt 技巧的人
- 文章讓「產品完整度」變成優點——你是在等工程完整性，而不是在慢

### 對方法論的影響

- 方法論不是「選項之一」，而是「自然結果」
- 防守位置轉換：不是要說服別人用這套方法論，是他們必須回答不用這套要怎麼處理這些問題
- 方法論會被視為「治理層」(Engineering Governance Layer)，不是「流程規範」

### 對投資者的影響

- 不會被當成「AI 工具創業者」，會被放進 Infrastructure / Governance / Control Plane 的估值邏輯
- 在投資者眼中會變成「定義問題的人」——先定義一個只會越來越痛的問題，再默默站在唯一能合理解釋它的位置

### 核心結論

> 這些文章不是在幫產品找市場，而是在幫市場準備好「理解這個產品所需的腦袋」。

---

## 教育成本與提早發布策略

### 核心觀點

> 提早不是為了治理需求，而是為了「教育成本曲線」。這套不是 prompt 演進線上的東西，越晚，認知修正成本越高。

### Prompt 路線的隱含教育模型

市場已形成的穩定心智模型：`需求 → prompt → LLM → 輸出 → 人修正`

隱性信念：Prompt 是臨時輸入、輸出是一次性結果、文件是輔助說明、問題靠再問一次解決。

而 AIGDMM 路線是另一條正交路線：`意圖 → 結構文件 → Contract → Execution → Trace / Audit → 再利用`

### 為什麼「越晚教育成本越高」

- 教育成本 = 破壞既有心智模型的成本，非使用難度
- Prompt 思維會被制度化（教學、流程、KPI、職稱），錯誤模型一旦被制度化，糾正成本是指數級上升
- 越晚越會被迫用「對方語言解釋自己」（「我們是更嚴謹的 prompt」——那一刻已經輸了）

### 正確的上市節奏

> 產品可以晚，但「世界觀不能晚」。現在要搶的不是用戶，而是詞彙權、分類權、問題定義權。

---

## Constraint vs Contract：工程必然性

### 模型不確定性（Model Uncertainty）

LLM 的不確定性來自三個層級：

1. **統計不確定性** — 機率取樣、temperature、top-k/top-p（同樣輸入 ≠ 同樣輸出）
2. **語意漂移不確定性** — 自然語言本身無精確邊界，模型合理地走向另一個語意空間
3. **任務邊界不確定性** — prompt 很難精確限制「不��做什麼」，對聊天是優點，對工程是災難

> LLM 是高創造性、不確定性系統，而工程系統的基本前提是行為可預期。

### Prompt 的三個上限

| 工程需要     | Prompt 能做到嗎 |
| ------------ | --------------- |
| 行為前驗證   | 否              |
| 行為邊界鎖定 | 否              |
| 行為責任歸屬 | 否              |

### Contract 的工程定義

> Contract = 可機器解析的約束集合 + 可驗證的前提條件 + 明確禁止的行為邊界 + 可稽核的執行結果對應

所有成熟工程領域都走過同樣的路：電力系統→保護繼電器、網路系統→TCP、軟體模組→Interface/Contract、金融系統→Risk limit。

### 錨點論述

> Prompt 是在「與模型對話」，Contract 是在「對模型設限」。當模型具備不可消除的不確定性時，任何嚴肅的工程系統，都不可能建立在對話之上，只能建立在約束之上。

---

## 第一篇文章：從 LLM 的本質看 Prompt 的天花板

### 文章定位與結構

- 不批評 AI、不批評使用者、不推銷任何方法，只指出一個工程不可能成立的前提
- 第一篇的任務：定義什麼是「不成立的做法」——否定 prompt 可以自然演進成工程介面、否定之後再補文件/治理就好、否定高手就不需要結構
- 把責任從「人」移回「結構」

### 六節結構

1. **LLM 的核心能力，來自不可消除的不確定性** — 機率分佈、統計推斷、語意取樣
2. **Prompt 的本質：不確定性輸入，而非約束機制** — 行為不可事前驗證、語意邊界不可封閉、結果不可追責
3. **當工程建立在 Prompt 之上，為什麼風險是不可逆的** — 路徑依賴：制度化→責任模糊→事故無補救接口
4. **時間維度的殘酷真相** — 程式碼能回答「做了什麼」，卻無法回答「為什麼」
5. **工程史的共同規律** — 不確定性一旦進入工程，就必須被 Constraint
6. **從 Prompt 走向 Contract，不是進階技巧，而是工程必然**

### Daily Engineer 比喻

每天由一位不同的高階工程師修改系統，每位只負責一天。Git diff 能看到改了什麼，但無法回答：
- 當時的設計意圖是什麼
- 有哪些選項被刻意排除
- 哪些風險是被評估後接受的
- 哪些行為是「被禁止」而非「被遺漏」

> 工程的本質不是某一個當下能不能運作，而是跨時間、跨人員、跨情境，仍然能被理解、維護與延續。

### 英文版標題

**The Ceiling of Prompts: Why LLM-Based Engineering Faces an Irreversible Crisis**

Subtitle: *Working code proves a system runs today. Engineering must prove it can evolve tomorrow.*

### 發布策略確認

- 第一篇應該發在 Medium（而非社群平台），因為這是會被引用、被回頭看的文章
- 適合 Markdown 貼上（Ctrl+Shift+V 純貼上）
- LinkedIn 只貼導讀 + 1-2 個關鍵句，連回全文

---

## 第二篇文章：效率放大與責任錯位

### 核心架構

使用者提出四點架構：
1. 想像一個人加上 LLM，十分鐘完成半天的工作，一天完成五至十人一週工作量，已是常態。但 AI 講 What，給的是 How，我們不知道 Why
2. 放大的是效率，省掉的是權責——一群 co-workers 本來承擔設計假設、風險質疑、理由留存等不同權責，效率壓縮後沒有被任何東西接手
3. 決策權的消失與 LLM 不可復現，How 不斷移轉，Why 永遠沒有答案
4. 引入 ISO/IEC 42001 作為制度化佐證

### 文章結構

1. **AI 回答了 How，但 Why 正在消失**
2. **被放大的不只是效率，而是權責被省略**
3. **決策權的消失，與不可復現的 How**
4. **三年後，沒有人能回答「為什麼是這樣」**
5. **這正是 ISO/IEC 42001 關心的核心問題** — 決策責任是否明確、行為是否可追溯、風險是否被識別與記錄、人類是否保有最終責任

### 核心論點

> 當執行能力被大幅放大，而責任結構沒有同步進化時，工程風險不是被解決，而是被延後。

---

## 第三篇文章：Trace Anchors 與 Constraint 角色

### 核心論點

- **Constraint 的強弱取決於「AI 什麼角色」** — Factory 模式需要強約束（穩定重複輸出），Explorer 模式需要少量精確約束（避免過早收斂）
- **Constraint 是唯一可用的槓桿** — 大多數人不是在訓練自己的 LLM，無法控制模型參數
- **Constraint 本身仍不夠** — 累積後會出現新問題：哪些 constraint 影響了決策？哪些是噪音？
- **Trace Anchors 先於 Control** — 回答「當時的意圖是什麼？哪些假設被接受？哪些替代方案被拒絕？」

### Explorer 與 Factory 的區分

- Factory：LLM 作為生產線，相同輸入應產出相同或高度相似結果，行為可預測
- Explorer：LLM 探索解空間，不確定性本身是價值的一部分
- 兩者都需要 Trace，但目的不同：Factory 需要「可重複」，Explorer 需要「路徑可理解」

### 迷宮比喻

探索就像老鼠走迷宮——學習只在記住哪些路是死路時發生。如果每次嘗試都不記錄，就沒有學習累積。探索不需要收斂到正確答案，但需要保留「路徑可理解性」。

---

## 第四篇文章：治理不是稽核

### 核心觀點

> 不是沒有解法，而是解法太多；第一篇不是情緒文，而是在做問題邊界的切割。

- 問題空間的凍結：先拒絕選邊、拒絕工具崇拜、拒絕解法競賽，堅持把問題空間畫乾淨
- 第四篇的價值：完成轉折——「第一篇不是因為悲觀而沒有解法，而是因為在那個階段，任何解法都會被誤用」
- Right Path 不是正確答案，而是「被記錄下來的選擇過程」

### 工程師風險 vs 世界觀定錨

如果先從「風險」談起，讀者會自動用既有世界觀解釋風險（「工程師要更小心」「AI 要加更多 review」）。正確順序是：先定義錯誤的工程假設，風險才變成推論結果而非情緒。

---

## 第五篇文章：治理必須左移

### 核心論點

> 治理的失敗，不是因為控制不夠，而是因為治理發生得太晚。如果治理只存在於事後，那就不是治理。

### 事中與事後的代價

- **事中被迫中斷**：AI 產生不可接受行為，工程師即時介入，工作節奏被持續打斷，團隊變成「AI 救火隊」
- **事後承擔審核責任**：AI 產出速度遠超人類理解能力，工程師轉為「產出審核者」，審核工作量隨 AI 效率同步放大，卻沒有結構幫助追上速度

### 與前四篇的連結

第五篇必須主動回扣前四篇的結論，讓治理需求「被逼出來」而非「被宣告」：
1. 不確定性無法消除（第一篇）
2. 權責結構被破壞（第二篇）
3. Constraint 有強弱情境差異（第三篇）
4. 沒有 trace 就沒有治理（第四篇）

### 工程史的類比

測試曾被放在最後，直到代價高昂的經驗告訴我們：品質只能在事後驗證時，不可能被系統性保證。測試被逐步左移，AI 治理正站在相同的轉折點上。

### 球給 AISCL 系列的三個需求點

1. 決策前提是否需要被清楚表達？
2. 哪些條件不能只存在於隱含假設中？
3. 哪些影響決策的因素必須能被看見、被對齊、被回顧？

---

## NNL (Normative Natural Language) 定義

### 概念起源

從「prompt engineering 的失誤」降階成一個「人類語言責任問題」：
> 不是 AI 沒聽懂，是人類沒有說清楚哪些東西不能誤會。

### 建議用詞 (Suggest Words)

must / must not, should, if…then…, on failure… — 這些不是裝飾詞，而是規範性載體 (normative carriers)

### 概念模式 (Conceptual Patterns)

- Pattern A — Goal: I want \<something\>.
- Pattern B — Important Condition: \<something\> must \<important behavior\>.
- Pattern C — Failure Handling: If \<condition\> fails, \<expected behavior\>.

把語句分類為描述性 (descriptive) 與規範性 (normative)。

### 為什麼 normative language 繞不開

一旦開始教人區分「重要的話」與「其他話」，就已經在做 normative language。後面所有延伸（NNL、AIGDCNL、Trace Anchor、Contract）都只是不同層級的實作。

### 關鍵特性

- **看一遍就會** — 不需要語法、關鍵字、工具、框架，讀完只多一個直覺：「這句話如果很重要，我應該說清楚一點」
- NNL 不是「知識」而是「責任直覺」
- 直接瓦解 prompt pattern engineer 的價值基礎：能力競賽 → 責任競爭

### 層級關係

```
Natural Language
   ↓
NNL (Normative Natural Language)
   ↓
AIGDCNL (AI-Governed / AI-Aware NNL)
   ↓
Contract / Execution / Trace Systems
```

### 命名權策略

- NNL 不綁 AI、不綁模型、不綁系統、不綁方法論、不綁產品
- 「normative」是學術與制度的重力井，未來即使出現其他產品名或 marketing 詞，嚴謹討論一定會收斂回來
- 就算別人用其他詞，始源一定會回到這篇 NNL — 因為定義的是「缺失層」而非「競爭詞」

### 發布時機

放在第四篇之後（第五篇之後），作為「入口校正篇」(Entry Correction)。太早發會被誤解為「另一種 prompt 技巧」或「比較高級的 prompt engineering」。

---

## 發布平台策略

### 平台角色分配

| 平台 | 適合放什麼 |
| ---- | ---------- |
| GitHub | README、schema、examples |
| Medium | 敘事、問題、工程反思（主要長文載體） |
| LinkedIn | 引流、討論、對話 |
| Substack | 個人 memo、時間線、給投資人的心路紀錄 |
| 學術平台 (arXiv/OSF/Zenodo) | 概念定義、立場論述 |

### Substack 定位

- 不是寫給演算法，是寫給未來
- 讀者 = 自我選擇留下的人（願意訂閱、長期觀察、回頭翻舊文）
- 天然是「時間線」：一個人長期思想與選擇的連續紀錄
- 適合發個人 memo（給對的投資人、也是給未來自己）

### 三層落點結構

```
LinkedIn → 問題、風險、工程直覺
Medium → 為什麼需要 AISCL（敘事）
Academic-style platform → What AISCL is / is not（概念定義）
GitHub → How it looks (NNL/CNL/Rule/Ruleset schema)
```

---

## 兩系列文章策略

### 系列一：Problem Series

**唯一 Topic：** When AI enters execution, engineering starts losing responsibility over time.

- 第一篇：Prompt 天花板
- 第二篇：效率放大與權責錯位
- 第三篇：Constraint 角色與 Trace Anchor
- 第四篇：治理不是稽核
- 第五篇：治理必須左移

> 系列一 = 問題不可逆性揭露

### 系列二：AISCL Series

**唯一 Topic：** The missing layer in AI-assisted engineering is not intelligence, but language.

#### 四篇結構（調整版）

1. **Why AI Governance Must Shift Left** — 系列入口，承接第一系列，把「Shift Left」引入治理
2. **Governance Happens at the Decision Layer** — 引出 DCL (Decision Constraint Layer) 的存在必要性
3. **Constraints Don't Govern Decisions — Anchors Do** — 把「治理」和「constraint 技巧」徹底分開
4. **From Shift-Left Governance to Constraint-First Design** — 收束，丟球給下一系列

#### 系列二邊界

- 只到 DCL（最多輕觸 AISCR 的需求面）
- 不講 devSCR、不講 schema、不講 prompt 寫法、不講工具或 pipeline

### 兩系列的關係

> 第一系列說清楚「問題為什麼存在」，第二系列要說清楚「治理應該站在哪一層思考」。

---

## DBG / DCL / AISCR / devSCR 分層架構

### 分層定義

```
Decision Behavior Governance   ← WHY（治理哲學）
└─ DCL (Decision Constraint Layer)  ← WHERE（架構層）
└─ AISCR (AI Structured Constraint Representation)  ← WHAT（約束表示層）
└─ devSCR  ← HOW（開發階段語言層 / syntax）
```

- **DBG**：Governance is about decision behavior, not models, prompts, or outputs
- **DCL**：治理發生在哪一層被治理（事前、不是事後）
- **AISCR**：Constraint 的「可被機器與治理理解的表示形式」（WHAT is represented, not HOW）
- **devSCR**：開發階段結構化表示，不是 runtime control language

### 各層出現時機

- 第二系列：只到 DCL
- 學術文：DBG + DCL + AISCR（需求面）
- 工程文章 / GitHub / 第三系列：devSCR

---

## AISCL 學術發表策略

### 適合的學術型自由發表平台

1. **arXiv** — 事實上的思想發表場，不需審稿，接受 Position Paper / Conceptual Framework
2. **OSF (Open Science Framework)** — 適合放「還不是標準，但希望被嚴肅對待」的概念
3. **Zenodo** — 可產生 DOI，可連 GitHub repo，適合放 AISCL 定義文

### 學術文正確定位

- 類型：Conceptual Framework / Position Paper / Foundational Language Proposal
- 不是新模型、新演算法、新 benchmark、Prompt engineering 技術
- 核心貢獻不是語法，而是「設計原則」

### 三篇學術文結構

1. **WHY** — 為什麼需要這個語言層（問題揭露、需求論證）
2. **WHAT** — 這個語言層必須滿足什麼條件（Constraint / Trace / Evidence 的需求表達）
3. **NAMING** — 語言層與治理的語義基礎（AISCL、Normative Intent、Trace Anchor 的命名）

### 需求 → Schema → 實作的單向關係

> 只有在需求被清楚陳述之後，schema 設計與實作才不是任意選擇，而是必然對應。

- 需求（WHY）：責任在時間維度上能否存在的必要條件
- Schema（WHAT）：對需求的結構化回應（至少必須長什麼樣子）
- 實作（HOW）：可替換、可演進

### 學術文需求寫作心法

> 學術需求不是在說「系統應該做什麼」，而是在說「如果做不到，工程一定會失敗」。

三個核心需求：
1. Constraint 必須可被記錄，否則在工程上等同不存在
2. AI 行為必須可被記錄，是在非決定性系統中責任存在的最低條件
3. Constraint 對 AI 行為的影響必須可被觀察，否則 Constraint 只能成為形式上的安慰

---

## 編輯檢查

- 是否依原始對話時間排序：是
- 是否保留所有有意義內容：是
- 是否只去除噪音，而非刪除語意：是
- 是否保留推論過程與被推翻的推論：是（包括第五篇「弱化前四篇連結」的被推翻版本）
- 是否將對話痕跡改寫為可閱讀的筆記結構：是
- 是否避免重複敘述同一內容：是
