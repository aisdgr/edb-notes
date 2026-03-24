<!--
Template Purpose:
- Transform one RAW conversation file into one NOTES file
- Preserve all meaningful content while removing filler, repetition, and chat artifacts
- Keep original conversation order

Conversion Rules:
- Only clean formatting noise, duplicated wording, filler, emotional feedback, and chat traces
- Do not delete invalid reasoning, later-retracted reasoning, or intermediate inference steps
- Do not add concepts not present in the RAW conversation
- Organize each conversation turn into a readable note section
- Use headings to structure content instead of keeping chat transcript style
-->

# {{conversation_title}}

## {{conversation_topic_1}}

### {{section_1_label}}

### {{section_1_topic_a}}
#### {{section_1_topic_a_subtopic_1}}
{{section_1_topic_a_subtopic_1_content}}

#### {{section_1_topic_a_subtopic_2}}
{{section_1_topic_a_subtopic_2_content}}

#### {{section_1_topic_a_subtopic_3}}
{{section_1_topic_a_subtopic_3_content}}

### {{section_1_topic_b}}
#### {{section_1_topic_b_subtopic_1}}
{{section_1_topic_b_subtopic_1_content}}

#### {{section_1_topic_b_subtopic_2}}
{{section_1_topic_b_subtopic_2_content}}

### {{section_1_topic_c}}
{{section_1_topic_c_content}}

### {{section_1_topic_d}}
{{section_1_topic_d_content}}

### {{section_1_topic_e}}
{{section_1_topic_e_content}}

---

## {{conversation_topic_2}}

### {{section_2_label}}

### {{section_2_topic_a}}
#### {{section_2_topic_a_subtopic_1}}
{{section_2_topic_a_subtopic_1_content}}

#### {{section_2_topic_a_subtopic_2}}
{{section_2_topic_a_subtopic_2_content}}

### {{section_2_topic_b}}
#### {{section_2_topic_b_subtopic_1}}
{{section_2_topic_b_subtopic_1_content}}

#### {{section_2_topic_b_subtopic_2}}
{{section_2_topic_b_subtopic_2_content}}

### {{section_2_topic_c}}
{{section_2_topic_c_content}}

### {{section_2_topic_d}}
{{section_2_topic_d_content}}

---

## {{conversation_topic_3}}

### {{section_3_label}}

### {{section_3_topic_a}}
{{section_3_topic_a_content}}

### {{section_3_topic_b}}
{{section_3_topic_b_content}}

### {{section_3_topic_c}}
{{section_3_topic_c_content}}

---

## 編輯檢查

- 是否依原始對話時間排序
- 是否保留所有有意義內容
- 是否只去除噪音，而非刪除語意
- 是否保留推論過程與被推翻的推論
- 是否將對話痕跡改寫為可閱讀的筆記結構
- 是否避免重複敘述同一內容

## 常用段落名稱參考

- 可依 row 回應實際內容選用，不必固定全部使用。

### 對話整體層級

- 文章分析
- 問題分析
- 主題分析
- 核心結論
- 總結
- 總結論
- 一句話總結

### 內容整理型

- 主要報導內容
- 背後的背景與事實脈絡
- 關鍵事實
- 核心概念
- 核心定義
- 定義
- 定位
- 目的
- 範圍
- 用法
- 哲學

### 結構拆解型

- 正確分界
- 正確分層
- 結構
- Canonical Rule 結構
- Ruleset 結構
- 責任切割
- 責任對照表
- 三層語意分離
- 四個核心構件
- 真正進入 execution 的只有三樣

### 評估判斷型

- 關鍵原則
- 關鍵要點
- 關鍵結論
- 關鍵 Insight
- Critical Anchor Point
- Core Conclusion
- Why RNL Effective
- 正確處理方式
- 正確定義方式

### 比較與取捨型

- 專家觀點
- 正確關係
- Correct Relationship
- 使用情境
- Usage Scenarios
- Common Error
- 不要在 ... 中放
- 應該在 ... 中放
- 不應該包含

### 風險與限制型

- 風險與限制
- Risk 處理原則
- 三種風險的區別
- 幫助有限的情境
- 不可逆限制
- 待補充部分
- 待強化部分

### 建議與落地方向

- Wording 建議
- 建議調整
- 最小侵入建議
- 實施建議
- 開發順序
- 品質控制
- 對產品的影響
- 對上市時間的影響

### 證據與案例型

- Example
- Example / Case
- 示例
- 例如
- 下表對比
- 主要貢獻
- 已完備部分

### 常見子段落名稱

- 特性
- 邊界
- 正確內容
- 不應該包含
- 目的
- 結論
- 原因
- 前提
- 影響
- 建議步驟