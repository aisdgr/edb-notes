# Skill Refactor Plan

## SKILL name
notes-to-knowledge

---

## 內容轉換流向

> RAW->NOTES->OUTLINE->CONCEPT->INDEX->POST

- 流程僅表示內容轉換的流向
- 內容轉換為單向
- 內容轉換流向並非全部都是以 POST 為終點
- 內容轉換流向不能以 Pipeline 方式進行

---

## 命令:

### 命令清單

- generate_notes
- generate_outline
- generate_concept
- generate_index
- generate_post

### 可以使用方式

- 指令模式: /generate_notes
- 提示詞模式: 
  - 轉換 row 為 notes
  - 生成 notes
  - 更新 notes

### 命令參數
- --interactive: 互動交談模式
- --agent: 代理模式(預設)

---

## 相關檔案

### config.yaml

> 環境參數設定

- 內容語系設定
- 內容檔案路徑
- 相關設定檔路徑
- 範本檔路徑
- 規則檔路徑
- 任務檔路徑
- 決策證據參數

### narrative-point.yaml

> 敘述要點設定檔

### viewpoint-{scenarion}.yaml

> 觀點結構設定檔

- 觀點使用的敘事要點
- 要描述在觀點下，整個敘事結構使用那些敘事要點，順序
- 每個敘事要點都要說明使用什麼觀點描述
- 一個觀點一個檔案

### templates/

> 輸出結構範本檔

#### content/

> 輸出內容範本

- notes.template.md
- outline.template.md
- concept.template.md
- index.template.md

#### evidence/

> 證據產出範本

- decision.template.yaml
- usage.template.json

---

## 概念產出流程

1. 產生及設定概念結構檔 `viewpoint-{scenarion}.yaml`
2. 產生概念內容 `viewpoint-xxx.md`
   - 依據指定的 `viewpoint-{scenarion}.yaml` 的敘事要點，敘事順序，敘事觀點產生
   - 概念內容由指定的 OUTLINE 補充進 viewpoint-xxx.md
   - 如果 viewpoint-xxx.md 中沒有可以從 OUTLINE 補充的內容，需標註 內容待補
   - 如果有可以從 OUTLINE 提取的內容，則同 OUTLINE 方式，僅標註原始來源的 NOTES 檔案位置，段落位置

---

## skill 檔共用結構

```markdown

## 任務說明

## 任務目標
- xxx
- xxx

## 規則約束
- xxxx.yaml

## 執行步驟

## 輸出內容範本
- xxx.template.md

## 輸出證據
- decision: OPEN
- usage: CLOSE
```