# SKILL 架構調整建議

**Generated:** 2026-03-27
**Source:** concept.suggest.md + folder.md
**Status:** Draft

---

## 一、現況分析

### 1.1 當前目錄結構

```
.claude/
├── commands/                    ← 獨立指令
│   ├── generate-notes.md
│   ├── generate-outline.md
│   ├── generate-concept.md      ← 待建立
│   ├── generate-index.md
│   └── generate-posts.md
└── skills/
    └── knowledge-evolution/
        └── pipeline/            ← Pipeline Skill
            ├── stages/
            └── SKILL.md

.aisdgr/
├── templates/                   ← 模板文件
├── rules/                       ← 轉換規則
├── schema/                      ← 結構定義
└── viewpoints/                  ← 觀點配置

configs/
└── viewpoints/                  ← 觀點配置（重複？）

.temp/
├── tasks/                       ← 任務與計劃
└── reports/                     ← 執行報告
```

### 1.2 問題診斷

**結構問題：**

| 問題                                              | 影響                          | 嚴重度 |
| ------------------------------------------------- | ----------------------------- | ------ |
| Commands 與 Skills 界線不清                       | 不確定何時用 command vs skill | 中     |
| `.aisdgr/viewpoints` 與 `configs/viewpoints` 重複 | 配置分散，維護困難            | 低     |
| Pipeline stages 未明確定義各階段檔案              | 難以擴充新階段（如 CONCEPT）  | 高     |
| 缺少統一的 stage 註冊機制                         | 新增階段需手動修改多處        | 中     |

**語義問題：**

| 問題                               | 影響                      | 嚴重度 |
| ---------------------------------- | ------------------------- | ------ |
| "generate-*" 命名暗示單向轉換      | 實際可能是雙向或迭代      | 低     |
| `knowledge-evolution` 名稱過於抽象 | 不清楚此 skill 的具體用途 | 中     |
| stages/ 目錄下缺少實際檔案         | 無法判斷現有實現          | 高     |

---

## 二、核心設計原則

### 2.1 Command vs Skill 的明確區分

**Command（指令）：**
- 定義：**單次執行的原子操作**
- 特性：無狀態、獨立、可直接調用
- 用途：快速轉換、查詢、驗證
- 例子：`/notes`, `/outline`, `/concept`

**Skill（技能）：**
- 定義：**可組合的多步驟工作流**
- 特性：有狀態、可編排、可調用其他 tools/commands
- 用途：複雜流程、跨階段整合、自動化
- 例子：`content-pipeline`, `knowledge-evolution`

**關係：**
```
Skill (orchestrator)
  └─> Command (step 1)
  └─> Command (step 2)
  └─> Command (step 3)
```

### 2.2 Pipeline 架構設計

**Pipeline 作為核心 Skill：**

```
content-pipeline (Skill)
├── stages/
│   ├── notes.yaml       ← NOTES 階段定義
│   ├── outline.yaml     ← OUTLINE 階段定義
│   ├── concept.yaml     ← CONCEPT 階段定義（新增）
│   ├── index.yaml       ← INDEX 階段定義
│   └── posts.yaml       ← POST 階段定義
├── templates/           ← 各階段模板
│   ├── notes.md
│   ├── outline.md
│   ├── concept.md       ← 新增
│   ├── index.md
│   └── posts.md
└── SKILL.md             ← Skill 主控文件
```

---

## 三、調整策略

### 3.1 Phase 1：統一目錄結構（立即）

#### 3.1.1 重組 Pipeline Skill

**目標：** 將 `knowledge-evolution/pipeline` 重構為 `content-pipeline`

**行動：**
```
.claude/skills/content-pipeline/    ← 重命名與提升
├── SKILL.md
├── stages/
│   ├── notes.yaml
│   ├── outline.yaml
│   ├── concept.yaml                ← 新增
│   ├── index.yaml
│   └── posts.yaml
├── templates/
│   ├── notes.md
│   ├── outline.md
│   ├── concept.md                  ← 新增
│   ├── index.md
│   └── posts.md
└── rules/
    └── (從 .aisdgr/rules 遷移)
```

**理由：**
- `content-pipeline` 名稱更明確（相比 knowledge-evolution）
- 集中管理 stages, templates, rules
- 減少跨目錄依賴

#### 3.1.2 整合 Commands

**目標：** 統一 command 與 stage 的調用方式

**方案 A（建議）：Command 作為 Stage 的快捷調用**
```markdown
# .claude/commands/generate-concept.md

---
skill: content-pipeline
stage: concept
---

呼叫 content-pipeline skill 的 concept 階段
```

**方案 B：Command 與 Stage 分離**
```
保留獨立 commands/，但實際執行時調用 pipeline
```

**建議採用方案 A**，理由：
- 避免邏輯重複
- 統一執行路徑
- 便於維護

#### 3.1.3 整合 Viewpoints

**目標：** 解決 `.aisdgr/viewpoints` 與 `configs/viewpoints` 重複

**建議：**
```
.aisdgr/viewpoints/        ← 保留（主要配置）
    └── (遷移 configs/viewpoints 內容至此)

configs/                   ← 刪除或重新定義用途
```

**理由：**
- `.aisdgr/` 是專案配置的標準位置
- 避免配置分散

---

### 3.2 Phase 2：建立 CONCEPT 支援（短期）

#### 3.2.1 建立 CONCEPT Stage 定義

**檔案：** `.claude/skills/content-pipeline/stages/concept.yaml`

```yaml
stage: CONCEPT
name: Concept Integration
description: 將 OUTLINE 轉換為有邊界的概念系統

input:
  - OUTLINE files (from previous stage)

output:
  - CONCEPT files (structured conceptual document)

trigger:
  command: /concept
  skill: content-pipeline

rules:
  - MUST define Concept Boundary first
  - MUST preserve conflicts, not resolve them
  - MUST establish narrative continuity
  - MUST classify statements by status
  - MUST separate in-scope from out-of-scope
  - MUST NOT prematurely converge
  - MUST NOT drop technical content

workflow:
  1. Load OUTLINE files
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

template: concept.md

quality_checks:
  - Boundary 是否明確定義？
  - 敘事是否連貫？
  - 衝突是否顯性化？
  - 分類是否完整 (C/Q/T/U)？
  - Out-of-Scope 是否清晰分離？
  - Evolution Backlog 是否可執行？
```

#### 3.2.2 建立 CONCEPT 模板

**檔案：** `.claude/skills/content-pipeline/templates/concept.md`

```markdown
# [Concept Title]

## Metadata
- **Version:**
- **Date:**
- **Status:** Draft / Evolving / Stabilized
- **Scope:**
- **Source:**

---

## 1. Concept Boundary

### In-Scope
<!-- 定義什麼屬於此概念系統 -->

### Out-of-Scope
<!-- 定義什麼不屬於此概念系統 -->

---

## 2. Problem Framing
<!-- 建立為何需要此概念的問題框架 -->

---

## 3. Core Narrative

### 3.1 Observed Phenomena
<!-- 來自 OUTLINE 的現象觀察 -->

### 3.2 Emerging Patterns
<!-- 從現象中浮現的模式 -->

### 3.3 Structural Tensions
<!-- 結構性衝突與張力 -->

### 3.4 Directional Hypotheses
<!-- 方向性假設 -->

---

## 4. Conceptual Structure
<!-- 概念骨架與層次結構 -->

---

## 5. Classified Statements

### 5.1 Established Conclusions
<!-- [C-XX] 已成立的結論 -->

### 5.2 Open Questions
<!-- [Q-XX] 待討論的問題 -->

### 5.3 Conflicts / Tensions
<!-- [T-XX] 衝突與張力 -->

### 5.4 Undefined / Ambiguous Zones
<!-- [U-XX] 未定義或模糊區域 -->

---

## 6. Implications

### 6.1 Engineering Implications
<!-- 對工程實踐的影響 -->

### 6.2 Governance Implications
<!-- 對治理的影響 -->

### 6.3 Research Implications
<!-- 對研究的影響 -->

---

## 7. Extraction Map

### → Paper Candidates
<!-- 可拆解為哪些論文主題 -->

### → Medium Topics
<!-- 可發表為哪些 Medium 文章 -->

### → Tool / Product
<!-- 可轉化為哪些工具或產品 -->

---

## 8. Out-of-Scope but Relevant Notes [O-XX]

### 8.1 Adjacent Concepts
<!-- 相鄰但屬於不同 Concept 的想法 -->

### 8.2 Cross-Domain Signals
<!-- 跨領域訊號 -->

### 8.3 Early Fragments
<!-- 尚未成熟的早期片段 -->

---

## 9. Evolution Backlog

### 9.1 TODO (Concept 完整性)
<!-- [TODO-XX] 必須補完的事項 -->

### 9.2 FEATURE (延伸能力)
<!-- [FEAT-XX] 可延伸的能力 -->

### 9.3 RESOURCING (資源/依賴)
<!-- [RES-XX] 推進所需資源 -->

---

## 10. Evolution Log

<!-- 概念演化記錄 -->
```

#### 3.2.3 建立 CONCEPT Command

**檔案：** `.claude/commands/generate-concept.md`

```markdown
---
skill: content-pipeline
stage: concept
---

# Generate CONCEPT

從 OUTLINE 生成有邊界的概念系統文件。

## 使用方式

```
/concept [options]
```

## 選項

- `--input <path>` : 指定 OUTLINE 檔案路徑
- `--output <path>` : 指定輸出路徑
- `--boundary <scope>` : 預先定義邊界範圍

## 執行流程

此指令會調用 `content-pipeline` skill 的 `concept` 階段：

1. 讀取 OUTLINE 檔案
2. 定義 Concept Boundary
3. 建構核心敘事
4. 分類陳述 (C/Q/T/U)
5. 生成 CONCEPT 文件
6. 產生執行報告

## 輸出

- `{topic}/concept/{version}.md` : CONCEPT 主文件
- `.temp/reports/concept-{date}.md` : 執行報告

## 相關指令

- `/outline` : 生成 OUTLINE（前置階段）
- `/index` : 生成 INDEX（後續階段）
```

---

### 3.3 Phase 3：增強 Pipeline 編排（中期）

#### 3.3.1 建立 Stage 註冊機制

**目標：** 支援動態註冊新階段

**檔案：** `.claude/skills/content-pipeline/registry.yaml`

```yaml
stages:
  - name: notes
    file: stages/notes.yaml
    command: /notes
    dependencies: []

  - name: outline
    file: stages/outline.yaml
    command: /outline
    dependencies: [notes]

  - name: concept
    file: stages/concept.yaml
    command: /concept
    dependencies: [outline]

  - name: index
    file: stages/index.yaml
    command: /index
    dependencies: [concept]

  - name: posts
    file: stages/posts.yaml
    command: /posts
    dependencies: [index]
```

#### 3.3.2 建立跨階段驗證

**目標：** 確保階段間的語義一致性

**檔案：** `.claude/skills/content-pipeline/validators/`

```yaml
# validators/boundary-consistency.yaml
name: Boundary Consistency Validator
description: 檢查 CONCEPT 與 OUTLINE 的邊界一致性

checks:
  - CONCEPT 的 In-Scope 是否有對應 OUTLINE 內容
  - OUTLINE 的重要內容是否被遺漏在 Out-of-Scope
  - 邊界定義是否前後一致
```

#### 3.3.3 建立增量更新機制

**目標：** 當 OUTLINE 更新時，僅更新受影響的 CONCEPT 部分

**實現：**
```yaml
# stages/concept.yaml (增補)
incremental_update:
  enabled: true
  trigger:
    - outline file changed

  strategy:
    - detect changed sections in OUTLINE
    - map to affected CONCEPT sections
    - regenerate only affected sections
    - preserve manual annotations
```

---

## 四、目標結構

### 4.1 調整後的目錄結構

```
.claude/
├── commands/                       ← 指令入口
│   ├── generate-notes.md           → 調用 pipeline/notes
│   ├── generate-outline.md         → 調用 pipeline/outline
│   ├── generate-concept.md         → 調用 pipeline/concept
│   ├── generate-index.md           → 調用 pipeline/index
│   └── generate-posts.md           → 調用 pipeline/posts
│
└── skills/
    └── content-pipeline/           ← 核心 Pipeline Skill
        ├── SKILL.md                ← Skill 主控文件
        ├── registry.yaml           ← Stage 註冊表
        │
        ├── stages/                 ← 各階段定義
        │   ├── notes.yaml
        │   ├── outline.yaml
        │   ├── concept.yaml        ← 新增
        │   ├── index.yaml
        │   └── posts.yaml
        │
        ├── templates/              ← 各階段模板
        │   ├── notes.md
        │   ├── outline.md
        │   ├── concept.md          ← 新增
        │   ├── index.md
        │   └── posts.md
        │
        ├── rules/                  ← 轉換規則（從 .aisdgr 遷移）
        │   ├── notes.yaml
        │   ├── outline.yaml
        │   ├── concept.yaml        ← 新增
        │   ├── index.yaml
        │   └── posts.yaml
        │
        └── validators/             ← 跨階段驗證器
            ├── boundary-consistency.yaml
            └── semantic-integrity.yaml

.aisdgr/
├── templates/                      ← 全域模板（保留）
├── rules/                          ← 全域規則（保留）
├── schema/                         ← 結構定義（保留）
└── viewpoints/                     ← 觀點配置（整合 configs/viewpoints）

configs/
└── (重新定義用途或刪除)

.temp/
├── tasks/                          ← 任務與計劃
└── reports/                        ← 執行報告
```

### 4.2 執行流程範例

**調用 `/concept` 指令：**

```
使用者輸入: /concept

↓
1. Command 識別
   generate-concept.md → skill: content-pipeline, stage: concept

↓
2. Skill 載入
   content-pipeline/SKILL.md 初始化

↓
3. Stage 解析
   載入 stages/concept.yaml
   檢查依賴：需要先執行 outline

↓
4. 規則應用
   載入 rules/concept.yaml
   應用轉換規則

↓
5. 模板渲染
   載入 templates/concept.md
   填充內容

↓
6. 品質檢查
   執行 quality_checks

↓
7. 輸出
   - {topic}/concept/{version}.md
   - .temp/reports/concept-{date}.md
```

---

## 五、實施優先級

### 5.1 優先級排序

| 優先級 | 任務                          | 原因                 | 預估時間 |
| ------ | ----------------------------- | -------------------- | -------- |
| **P0** | 建立 concept.yaml, concept.md | CONCEPT 階段核心需求 | 1-2 小時 |
| **P0** | 建立 generate-concept.md      | 提供指令入口         | 0.5 小時 |
| **P1** | 重組為 content-pipeline       | 統一架構，便於擴充   | 2-3 小時 |
| **P1** | 整合 viewpoints 配置          | 解決配置分散問題     | 1 小時   |
| **P1** | 遷移 rules 到 pipeline        | 集中管理             | 1 小時   |
| **P2** | 建立 registry.yaml            | 支援動態註冊         | 1 小時   |
| **P2** | 建立 validators               | 提升品質             | 2 小時   |
| **P3** | 增量更新機制                  | 效能優化             | 3-4 小時 |

### 5.2 時間規劃

| 階段               | 工作項  | 建議時間 |
| ------------------ | ------- | -------- |
| **Phase 1** (立即) | P0 任務 | Day 1    |
| **Phase 2** (短期) | P1 任務 | Day 2-3  |
| **Phase 3** (中期) | P2 任務 | Week 1   |
| **Phase 4** (長期) | P3 任務 | Week 2-3 |

---

## 六、風險與緩解

### 6.1 風險識別

| 風險                      | 影響 | 機率 | 緩解措施                       |
| ------------------------- | ---- | ---- | ------------------------------ |
| 重組破壞現有功能          | 高   | 中   | 先備份，逐階段遷移，保留舊路徑 |
| Command 與 Stage 調用衝突 | 中   | 低   | 統一調用介面，避免重複實現     |
| 配置遷移遺漏              | 中   | 中   | 建立檢查清單，驗證遷移完整性   |
| 過度設計導致複雜化        | 高   | 中   | 優先實現核心功能，逐步擴充     |

### 6.2 回滾計劃

**若新架構失敗：**
1. 保留舊目錄結構的備份
2. 可快速回滾到 `knowledge-evolution` 架構
3. CONCEPT 功能可獨立使用（不依賴 pipeline）

---

## 七、關鍵決策

### 7.1 決策 1：Pipeline 名稱

**選項：**
- A. `knowledge-evolution` (保留)
- B. `content-pipeline` (建議)
- C. `chat-to-paper`

**建議：B**
- 理由：`content-pipeline` 更明確表達功能
- `knowledge-evolution` 過於抽象
- `chat-to-paper` 過於具體（可能支援其他輸出）

### 7.2 決策 2：Rules 位置

**選項：**
- A. 保留在 `.aisdgr/rules/`
- B. 遷移到 `.claude/skills/content-pipeline/rules/`
- C. 兩者並存

**建議：B（階段性）**
- 短期：遷移到 pipeline/rules/ 便於管理
- 長期：`.aisdgr/rules/` 作為全域規則覆蓋

### 7.3 決策 3：Command 是否必要

**選項：**
- A. 保留獨立 commands/
- B. 取消 commands/，直接使用 skill
- C. Commands 作為快捷調用

**建議：C**
- 理由：提供使用者友善的入口
- 實際執行仍由 skill 處理
- 避免邏輯重複

---

## 八、成功標準

### 8.1 Phase 1 成功標準

- ✅ `/concept` 指令可正常調用
- ✅ 生成符合 10 章節結構的 CONCEPT 文件
- ✅ 品質檢查通過率 > 80%

### 8.2 Phase 2 成功標準

- ✅ 目錄結構重組完成
- ✅ 所有階段使用統一架構
- ✅ 配置整合完成（無重複）

### 8.3 Phase 3 成功標準

- ✅ 新增階段只需修改 registry.yaml
- ✅ 跨階段驗證機制運作
- ✅ 文檔完整性 > 90%

---

## 九、下一步行動

### 9.1 立即執行 (Today)

- [ ] 建立 `stages/concept.yaml`
- [ ] 建立 `templates/concept.md`
- [ ] 建立 `commands/generate-concept.md`
- [ ] 測試 `/concept` 指令

### 9.2 本週執行

- [ ] 重組為 `content-pipeline` 架構
- [ ] 遷移 rules 到 pipeline/
- [ ] 整合 viewpoints 配置
- [ ] 更新 CLAUDE.md 說明

### 9.3 持續改進

- [ ] 建立 registry.yaml
- [ ] 實現跨階段驗證
- [ ] 建立增量更新機制
- [ ] 優化效能與錯誤處理

---

## 十、參考資料

### 10.1 相關文件

- `task/concept.md` - CONCEPT 原始對話記錄
- `task/concept.plan.md` - CONCEPT 結構調整計劃
- `.claude/CLAUDE.md` - 系統主控文件
- `.claude/skills/knowledge-evolution/pipeline/SKILL.md` - 現有 Pipeline 實現

### 10.2 設計原則總結

1. **單一職責：** Command 負責入口，Skill 負責編排
2. **統一架構：** 所有階段使用一致的結構
3. **可擴充性：** 新增階段只需修改 registry
4. **品質優先：** 內建驗證與檢查機制
5. **使用者友善：** 提供簡單的指令入口

---

**End of Suggestion**
