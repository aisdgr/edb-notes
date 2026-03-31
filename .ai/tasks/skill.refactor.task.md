# Skill Refactor Task Plan

> **基於**: `skill.refactor.analysis.md` 決策結果
> **目標**: 將 `notes-to-knowledge` skill 重構為符合決策規範的架構
> **日期**: 2026-03-31
> **版本**: v1.0

---

## 一、重構總覽

### 1.1 核心決策摘要

| 決策項目      | 決議內容                                        | 優先級 |
| ------------- | ----------------------------------------------- | ------ |
| CONCEPT 定位  | 獨立階段，負責觀點重組，僅為中繼                | P0     |
| Pipeline 限制 | 禁用鏈式自動執行，僅允許單步轉換                | P0     |
| 階段轉換      | 僅允許相鄰階段轉換                              | P0     |
| 執行模式      | 互動模式（逐步確認）vs 代理模式（全自動，預設） | P1     |
| 配置優先級    | 全局 > 專案 > 用戶                              | P1     |
| 模板引擎      | Handlebars                                      | P1     |
| 證據存儲      | 由 config 決定（All/No/Success/Fail）           | P1     |
| 配置加密      | 需要加密或脫敏                                  | P2     |
| 多語言        | config 設定 + 命令列指定                        | P2     |

### 1.2 重構階段

```
Phase 1: 核心架構 (P0) → Phase 2: 功能實現 (P1) → Phase 3: 優化增強 (P2)
```

---

## 二、重構任務清單

### Phase 1: 核心架構重構 (P0)

#### Task 1.1: 階段系統重構

**目標**: 實現嚴格的相鄰階段轉換控制

**變更內容**:
- [ ] 更新 `.claude/skills/content-pipeline/skill.yaml`
  - 移除跨階段轉換支援
  - 新增階段驗證邏輯
  - 定義階段順序：`raw → notes → outline → concept → index → post`

**實作位置**:
```
.claude/skills/content-pipeline/
├── skill.yaml                    # 更新階段定義
├── stages/
│   ├── notes.yaml               # 新增 prev_stage: raw, next_stage: outline
│   ├── outline.yaml             # 新增 prev_stage: notes, next_stage: concept
│   ├── concept.yaml             # 新增 prev_stage: outline, next_stage: index
│   ├── index.yaml               # 新增 prev_stage: concept, next_stage: post
│   └── post.yaml                # 新增 prev_stage: index, next_stage: null
└── validators/
    └── stage-transition.yaml    # 新增階段轉換驗證規則
```

**驗證標準**:
- 嘗試 NOTES→CONCEPT 應報錯
- 僅允許 NOTES→OUTLINE

**執行命令**:
```bash
# 1. 備份現有配置
cp .claude/skills/content-pipeline/skill.yaml .claude/skills/content-pipeline/skill.yaml.bak

# 2. 更新階段定義
# (手動編輯 skill.yaml，新增階段約束)

# 3. 驗證階段轉換
claude-code /notes --test-transition raw notes      # 應成功
claude-code /notes --test-transition notes outline  # 應成功
claude-code /notes --test-transition notes concept  # 應失敗
```

---

#### Task 1.2: Pipeline 控制機制

**目標**: 禁用鏈式自動執行，強制單步轉換

**變更內容**:
- [ ] 新增 `pipeline.mode: stepwise` 配置
- [ ] 移除任何 `RAW→NOTES→OUTLINE` 自動鏈式邏輯
- [ ] 每次執行後停止，需明確指定下一步

**實作位置**:
```
config.yaml
├── pipeline:
│   ├── mode: "stepwise"          # 新增：stepwise | chain(auto)
│   ├── allow_chain: false        # 新增：禁用鏈式
│   └── max_chain_depth: 1        # 新增：最大深度=1
```

**配置範例**:
```yaml
# config.yaml
pipeline:
  mode: "stepwise"           # 強制單步模式
  allow_chain: false         # 禁用鏈式執行
  max_chain_depth: 1         # 僅允許深度=1
  auto_trigger:
    enable: false            # 禁用自動觸發
```

**驗證標準**:
- 執行 `/notes` 後應停止，不自動觸發 `/outline`
- 嘗試 `/notes --chain` 應報錯或被忽略

**執行命令**:
```bash
# 1. 更新 config.yaml
# (新增 pipeline 配置區塊)

# 2. 測試單步執行
claude-code /notes raw/20260331_001.md
# 應僅生成 notes，不自動執行 outline

# 3. 測試禁用鏈式
claude-code /notes raw/20260331_001.md --chain
# 應報錯或忽略 --chain 參數
```

---

#### Task 1.3: 資料流向架構實現

**目標**: 實現 RAW→NOTES→OUTLINE→CONCEPT 的正確聚合邏輯

**決議依據**:
- 一個 RAW → 一個 NOTES
- 多個 NOTES → 一個 OUTLINE（聚合）
- OUTLINE 為最大集合
- 一個 OUTLINE → 多個 CONCEPT（不同 viewpoint）

**變更內容**:
- [ ] 更新 OUTLINE 生成邏輯，支援多 NOTES 聚合
- [ ] 更新 CONCEPT 生成邏輯，支援單一 OUTLINE 產出多個 CONCEPT
- [ ] 新增聚合規則配置

**實作位置**:
```
.claude/skills/content-pipeline/
├── stages/
│   ├── outline.yaml
│   │   └── aggregation:
│   │       ├── enable: true
│   │       ├── sources: "notes/*.md"
│   │       └── strategy: "merge_deduplicate"
│   └── concept.yaml
│       └── generation:
│           ├── source: "outline/*.md"  # 單一來源
│           ├── multiple_output: true    # 支援多輸出
│           └── viewpoint_based: true    # 基於 viewpoint
└── rules/
    └── aggregation.yaml         # 新增聚合規則
```

**配置範例**:
```yaml
# stages/outline.yaml
aggregation:
  enable: true
  sources:
    - "notes/*.md"
  strategy: "merge_deduplicate"  # 合併並去重
  output:
    single_file: true            # 聚合為單一 OUTLINE
    filename_pattern: "outline_{date}_{hash}.md"

# stages/concept.yaml
generation:
  source:
    type: "single"               # 單一 OUTLINE
    path: "outline/*.md"
  output:
    multiple: true               # 多個 CONCEPT
    viewpoint_driven: true       # 由 viewpoint 驅動
```

**驗證標準**:
- 3 個 NOTES 應聚合為 1 個 OUTLINE
- 1 個 OUTLINE + 2 個 viewpoint 應產生 2 個 CONCEPT

**執行命令**:
```bash
# 1. 準備測試數據
echo "# RAW 1" > raw/test_001.md
echo "# RAW 2" > raw/test_002.md
echo "# RAW 3" > raw/test_003.md

# 2. 生成 NOTES
claude-code /notes raw/test_001.md
claude-code /notes raw/test_002.md
claude-code /notes raw/test_003.md

# 3. 聚合為 OUTLINE
claude-code /outline notes/test_*.md
# 應生成單一 outline/test_outline.md

# 4. 生成多個 CONCEPT
claude-code /concept outline/test_outline.md --viewpoint technical
claude-code /concept outline/test_outline.md --viewpoint business
# 應生成 concept/test_technical.md, concept/test_business.md
```

---

### Phase 2: 功能實現 (P1)

#### Task 2.1: 命令系統重構

**目標**: 簡化命令為 `/notes`, `/outline`, `/concept`, `/index`, `/post`

**變更內容**:
- [ ] 重新命名命令（移除 `generate_` 前綴）
- [ ] 新增命令別名支援
- [ ] 新增執行模式參數

**實作位置**:
```
.claude/skills/content-pipeline/
└── commands/
    ├── notes.yaml
    ├── outline.yaml
    ├── concept.yaml
    ├── index.yaml
    └── post.yaml
```

**配置範例**:
```yaml
# commands/notes.yaml
name: "notes"
aliases: ["gn", "gen-notes"]
description: "從 RAW 生成 NOTES"
parameters:
  - name: "interactive"
    short: "i"
    type: "boolean"
    default: false
    description: "互動模式：逐步確認"
  - name: "agent"
    short: "a"
    type: "boolean"
    default: true
    description: "代理模式：全自動執行（預設）"
  - name: "input"
    type: "path"
    required: true
    description: "RAW 檔案路徑"
execution:
  mode_default: "agent"          # 預設代理模式
  interactive:
    - step_by_step: true
    - confirm_decisions: true
    - review_output: true
  agent:
    - auto_execute: true
    - auto_error_handling: true
    - notify_on_complete: true
```

**驗證標準**:
- `/notes` 應等同於舊 `generate_notes`
- `/notes -i` 應進入互動模式
- `/notes --agent` 應全自動執行

**執行命令**:
```bash
# 1. 測試命令簡化
claude-code /notes raw/test.md          # 應成功
claude-code /gn raw/test.md             # 應成功（別名）

# 2. 測試互動模式
claude-code /notes raw/test.md -i       # 應逐步確認

# 3. 測試代理模式
claude-code /notes raw/test.md --agent  # 應全自動執行
```

---

#### Task 2.2: 配置繼承系統

**目標**: 實現三層配置繼承（全局 > 專案 > 用戶）

**變更內容**:
- [ ] 新增配置層級定義
- [ ] 實現配置優先級邏輯
- [ ] 新增衝突檢測與提示

**實作位置**:
```
config/
├── global.yaml                  # 全局配置（最高優先級）
├── project.yaml                 # 專案配置
└── user.yaml                    # 用戶配置（最低優先級）
```

**配置範例**:
```yaml
# config/global.yaml
metadata:
  level: "global"
  priority: 100

pipeline:
  mode: "stepwise"
  allow_chain: false

# config/project.yaml
metadata:
  level: "project"
  priority: 50

output:
  language: "zh-TW"

# config/user.yaml
metadata:
  level: "user"
  priority: 10

personal:
  author: "user_name"
```

**合併邏輯**:
```python
# 優先級：global > project > user
# 衝突處理：檢測到衝突時提示使用者選擇

def merge_configs():
    configs = [load_global(), load_project(), load_user()]
    merged = {}

    for cfg in configs:
        for key, value in cfg.items():
            if key in merged and merged[key] != value:
                # 檢測到衝突
                choice = prompt_user(f"配置衝突: {key}\n1. {merged[key]} (優先級高)\n2. {value} (優先級低)\n請選擇: ")
                if choice == "2":
                    merged[key] = value
            else:
                merged[key] = value

    return merged
```

**驗證標準**:
- global.yaml 配置應覆蓋 project.yaml
- 衝突時應提示使用者選擇

**執行命令**:
```bash
# 1. 建立配置檔案
mkdir -p config
touch config/global.yaml config/project.yaml config/user.yaml

# 2. 測試配置合併
claude-code /notes --show-config
# 應顯示合併後的配置

# 3. 測試衝突處理
# (在 global.yaml 和 project.yaml 設定相同 key 但不同 value)
claude-code /notes
# 應提示衝突並要求選擇
```

---

#### Task 2.3: 模板引擎遷移

**目標**: 遷移至 Handlebars 模板引擎

**變更內容**:
- [ ] 安裝 Handlebars 依賴
- [ ] 更新所有模板為 Handlebars 語法
- [ ] 新增模板驗證機制

**實作位置**:
```
templates/
├── content/
│   ├── notes.template.md
│   ├── outline.template.md
│   ├── concept.template.md
│   └── index.template.md
└── evidence/
    ├── decision.template.yaml
    └── usage.template.json
```

**模板範例**:
```markdown
<!-- templates/content/notes.template.md -->
---
required:
  - title
  - key_points
optional:
  - author: "佚名"
  - date: "{{current_date}}"
---

# {{title}}

{{#if author}}
**作者**: {{author}}
{{/if}}

**日期**: {{date}}

## 關鍵要點

{{#each key_points}}
### {{@index}}. {{this.title}}

{{this.content}}

{{#if this.source}}
> 來源: {{this.source}}
{{/if}}

{{/each}}

## 標籤

{{#each tags}}
- {{this}}
{{/each}}
```

**驗證標準**:
- 模板應支援變量、條件、循環
- 缺少必填字段應報錯

**執行命令**:
```bash
# 1. 安裝 Handlebars (假設使用 Python)
pip install pybars3

# 2. 遷移模板
# (手動更新所有 .template.md 檔案)

# 3. 驗證模板
claude-code /notes --validate-template templates/content/notes.template.md

# 4. 測試模板渲染
claude-code /notes raw/test.md --dry-run
# 應正確渲染 Handlebars 模板
```

---

#### Task 2.4: 敘述要點系統重構

**目標**: 實現抽象要點 + Qualifier 分離

**決議依據**:
- `narrative-point.yaml` 定義抽象要點（不帶 Qualifier）
- `viewpoint-xxx.yaml` 加上 Qualifier 描述

**變更內容**:
- [ ] 重構 `narrative-point.yaml` 為抽象要點
- [ ] 更新 `viewpoint-xxx.yaml` 加上 Qualifier
- [ ] 新增要點完整性檢查（模板引導）

**實作位置**:
```
config/
├── narrative-point.yaml         # 抽象要點定義
└── viewpoints/
    ├── technical.yaml           # 技術觀點（加 Qualifier）
    ├── business.yaml            # 商業觀點（加 Qualifier）
    └── base.yaml                # 基礎觀點（可繼承）
```

**配置範例**:
```yaml
# narrative-point.yaml (抽象要點)
points:
  - id: "problem"
    description: "問題描述"
    required: true

  - id: "solution"
    description: "解決方案"
    required: true

  - id: "result"
    description: "結果與影響"
    required: false

# viewpoints/technical.yaml (加 Qualifier)
extends: base.yaml
viewpoint:
  name: "technical"
  description: "技術視角"
points:
  - id: "problem"
    qualifier: "技術"
    description: "技術層面的問題描述"
    perspective: "從技術架構、性能、可維護性角度描述問題"

  - id: "solution"
    qualifier: "工程"
    description: "工程化解決方案"
    perspective: "從設計模式、技術選型、實作細節角度描述解決方案"

# viewpoints/business.yaml (加 Qualifier)
viewpoint:
  name: "business"
  description: "商業視角"
points:
  - id: "problem"
    qualifier: "業務"
    description: "業務層面的問題描述"
    perspective: "從業務流程、成本、效率角度描述問題"
```

**驗證標準**:
- 抽象要點不應包含 Qualifier
- Viewpoint 應包含完整的 Qualifier 描述
- 缺少必要要點應警告

**執行命令**:
```bash
# 1. 備份現有配置
cp config/narrative-point.yaml config/narrative-point.yaml.bak

# 2. 重構 narrative-point.yaml
# (移除所有 Qualifier，保留抽象要點)

# 3. 建立 viewpoint 配置
mkdir -p config/viewpoints
touch config/viewpoints/technical.yaml
touch config/viewpoints/business.yaml

# 4. 驗證要點完整性
claude-code /concept --validate-points config/viewpoints/technical.yaml
# 應檢查所有必要要點都已定義
```

---

#### Task 2.5: 決策證據系統

**目標**: 實現可配置的證據存儲策略

**決議依據**:
- 由 config 決定存儲策略：All / No / Success / Fail
- 支援查詢：基本搜索 + 全文搜索 + 標籤搜索

**變更內容**:
- [ ] 新增證據存儲策略配置
- [ ] 實現證據索引機制
- [ ] 新增證據搜索功能

**實作位置**:
```
evidence/
├── decisions/
│   ├── 2026-03-31/
│   │   ├── decision_001.yaml
│   │   └── decision_002.yaml
│   └── index.yaml              # 證據索引
└── usage/
    └── usage_2026-03-31.json
```

**配置範例**:
```yaml
# config.yaml
evidence:
  storage: "configurable"        # configurable | always | never
  strategy: "all"                # all | no | success | fail
  retention_days: 90
  search:
    enable: true
    types:
      - basic                    # ID/階段/時間
      - fulltext                 # 全文搜索
      - tags                     # 標籤搜索
  index:
    enable: true
    auto_update: true
```

**證據範例**:
```yaml
# evidence/decisions/2026-03-31/decision_001.yaml
decision_id: "dec_20260331_001"
timestamp: "2026-03-31T10:30:00Z"
stage: "notes"
input:
  source: "raw/test_001.md"
  checksum: "sha256:abc123..."
output:
  target: "notes/test_001.md"
  checksum: "sha256:def456..."
reasoning:
  - step: 1
    action: "extract_key_points"
    result: "提取了 5 個關鍵要點"
  - step: 2
    action: "select_template"
    result: "選擇 notes.template.md"
status: "success"
tags: ["auto", "notes", "success"]
```

**驗證標準**:
- `strategy: all` 應保留所有證據
- `strategy: fail` 僅保留失敗證據
- 搜索功能應支援 ID/階段/時間/標籤

**執行命令**:
```bash
# 1. 更新 config.yaml
# (新增 evidence 配置區塊)

# 2. 測試證據存儲 - All
claude-code /notes raw/test.md
# 應生成 evidence/decisions/xxx.yaml

# 3. 測試證據搜索
claude-code /evidence search --stage notes --date 2026-03-31
claude-code /evidence search --fulltext "關鍵要點"
claude-code /evidence search --tags auto,success
```

---

### Phase 3: 優化增強 (P2)

#### Task 3.1: 配置加密機制

**目標**: 實現配置文件加密或脫敏

**變更內容**:
- [ ] 新增敏感字段識別
- [ ] 實現加密/脫敏邏輯
- [ ] 新增解密授權機制

**實作位置**:
```
config/
├── global.yaml
├── global.yaml.encrypted        # 加密版本
└── secrets.yaml                 # 敏感資訊（加密存儲）
```

**配置範例**:
```yaml
# config.yaml
encryption:
  enable: true
  algorithm: "AES-256-GCM"
  sensitive_fields:
    - "api_keys.*"
    - "credentials.*"
    - "secrets.*"
  key_management: "env"          # env | file | vault
  key_env_var: "CLAUDE_CONFIG_KEY"
```

**執行命令**:
```bash
# 1. 設定加密金鑰
export CLAUDE_CONFIG_KEY="your-encryption-key"

# 2. 加密配置
claude-code /config encrypt config/global.yaml

# 3. 解密配置（需授權）
claude-code /config decrypt config/global.yaml.encrypted
```

---

#### Task 3.2: 多語言支援

**目標**: 支援 config 設定產出語系 + 命令列強制指定

**變更內容**:
- [ ] 新增語系配置
- [ ] 實現多語言模板
- [ ] 新增命令列語系參數

**配置範例**:
```yaml
# config.yaml
output:
  language: "zh-TW"              # zh-TW | zh-CN | en-US | ja-JP
  fallback: "en-US"              # 回退語言

# 命令列覆蓋
claude-code /notes raw/test.md --language en-US
```

**執行命令**:
```bash
# 1. 設定預設語系
# (在 config.yaml 設定 output.language)

# 2. 測試預設語系
claude-code /notes raw/test.md
# 應使用 zh-TW 輸出

# 3. 測試命令列覆蓋
claude-code /notes raw/test.md --language en-US
# 應強制使用 en-US 輸出
```

---

#### Task 3.3: 質量評分系統

**目標**: 實現內容質量評分（完整性、一致性、可讀性、原創性）

**變更內容**:
- [ ] 新增質量評分引擎
- [ ] 定義評分維度與權重
- [ ] 生成質量報告

**實作位置**:
```
quality/
├── scorer.yaml                  # 評分配置
└── reports/
    └── quality_2026-03-31.md    # 評分報告
```

**配置範例**:
```yaml
# quality/scorer.yaml
dimensions:
  - name: "完整性"
    weight: 0.3
    checks:
      - "all_required_points_covered"
      - "no_missing_sections"

  - name: "一致性"
    weight: 0.25
    checks:
      - "no_contradictory_statements"
      - "consistent_terminology"

  - name: "可讀性"
    weight: 0.25
    checks:
      - "clear_structure"
      - "smooth_language"

  - name: "原創性"
    weight: 0.2
    checks:
      - "not_direct_copy"
      - "added_insights"
```

**執行命令**:
```bash
# 1. 評分內容
claude-code /notes raw/test.md --quality-score

# 2. 生成質量報告
claude-code /quality report --date 2026-03-31
```

---

## 三、執行順序建議

### 推薦執行流程

```
Week 1: Phase 1 (P0)
├─ Day 1-2: Task 1.1 階段系統重構
├─ Day 3-4: Task 1.2 Pipeline 控制機制
└─ Day 5:   Task 1.3 資料流向架構

Week 2-3: Phase 2 (P1)
├─ Day 1-2: Task 2.1 命令系統重構
├─ Day 3-4: Task 2.2 配置繼承系統
├─ Day 5-6: Task 2.3 模板引擎遷移
├─ Day 7-8: Task 2.4 敘述要點系統
└─ Day 9-10: Task 2.5 決策證據系統

Week 4: Phase 3 (P2)
├─ Day 1-2: Task 3.1 配置加密機制
├─ Day 3-4: Task 3.2 多語言支援
└─ Day 5:   Task 3.3 質量評分系統
```

---

## 四、驗證檢查清單

### Phase 1 驗證

- [ ] 階段轉換驗證：僅允許相鄰階段
- [ ] Pipeline 驗證：禁用鏈式自動執行
- [ ] 資料流向驗證：多 NOTES → 單 OUTLINE → 多 CONCEPT

### Phase 2 驗證

- [ ] 命令驗證：`/notes`, `/outline` 等命令正常
- [ ] 配���繼承驗證：全局 > 專案 > 用戶
- [ ] 模板驗證：Handlebars 語法正確
- [ ] 敘述要點驗證：抽象要點 + Qualifier 分離
- [ ] 證據驗證：可配置存儲策略

### Phase 3 驗證

- [ ] 加密驗證：敏感字段已加密
- [ ] 多語言驗證：支援 zh-TW/en-US 切換
- [ ] 質量評分驗證：四維度評分正常

---

## 五、回滾計劃

### 回滾策略

每個 Task 執行前應：
1. 備份現有檔案（`.bak` 後綴）
2. 記錄變更日誌
3. 提供回滾命令

**回滾命令範例**:
```bash
# 回滾 Task 1.1
claude-code /rollback --task 1.1

# 回滾至特定時間點
claude-code /rollback --date 2026-03-30

# 回滾整個 Phase
claude-code /rollback --phase 1
```

---

## 六、風險評估

| 風險                 | 影響 | 概率 | 緩解措施                   |
| -------------------- | ---- | ---- | -------------------------- |
| 階段轉換破壞現有流程 | 高   | 中   | 充分測試，保留向後兼容模式 |
| Handlebars 遷移失敗  | 中   | 低   | 分階段遷移，保留舊模板     |
| 配置繼承複雜度過高   | 中   | 中   | 限制繼承層級（最多3層）    |
| 加密金鑰遺失         | 高   | 低   | 提供金鑰備份機制           |

---

## 七、完成標準

### Definition of Done (DoD)

每個 Task 完成標準：
- [ ] 程式碼已實作並提交
- [ ] 單元測試通過
- [ ] 集成測試通過
- [ ] 文檔已更新
- [ ] 變更日誌已記錄
- [ ] Code Review 通過
- [ ] 使用者驗收通過

---

**生成時間**: 2026-03-31
**預計完成**: 2026-04-28 (4週)
**負責人**: TBD
**下一步**: 開始執行 Task 1.1 階段系統重構
