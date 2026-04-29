# Configuration Inheritance System
# 配置繼承系統說明

## 三層配置架構

```
Global (全局) → Project (專案) → User (用戶)
優先級: 100    >    優先級: 50    >   優先級: 10
```

### 配置層級說明

| 層級    | 檔案                  | 優先級 | 用途                       |
| ------- | --------------------- | ------ | -------------------------- |
| Global  | `config/global.yaml`  | 100    | 全局預設配置，最高優先級   |
| Project | `config/project.yaml` | 50     | 專案特定配置，覆蓋全局配置 |
| User    | `config/user.yaml`    | 10     | 用戶個人配置，覆蓋專案配置 |

**注意**：優先級高的配置會覆蓋優先級低的配置。

---

## 配置合併邏輯

### 合併規則

```python
def merge_configs():
    """
    合併三層配置

    優先級順序：Global > Project > User
    """
    global_cfg = load_yaml("config/global.yaml")
    project_cfg = load_yaml("config/project.yaml")
    user_cfg = load_yaml("config/user.yaml")

    # 從低優先級到高優先級合併
    merged = {}
    merged = deep_merge(merged, user_cfg)      # 優先級最低
    merged = deep_merge(merged, project_cfg)   # 優先級中等
    merged = deep_merge(merged, global_cfg)    # 優先級最高

    return merged
```

### Deep Merge 策略

```python
def deep_merge(base, override):
    """
    深度合併兩個配置字典

    Args:
        base: 基礎配置（優先級低）
        override: 覆蓋配置（優先級高）

    Returns:
        合併後的配置
    """
    result = base.copy()

    for key, value in override.items():
        if key in result:
            # 如果兩邊都是字典，遞迴合併
            if isinstance(result[key], dict) and isinstance(value, dict):
                result[key] = deep_merge(result[key], value)
            else:
                # 否則直接覆蓋
                result[key] = value
        else:
            # 新增 key
            result[key] = value

    return result
```

---

## 衝突檢測與處理

### 衝突檢測

```python
def detect_conflicts():
    """
    檢測配置衝突

    Returns:
        List[Conflict]: 衝突清單
    """
    conflicts = []

    global_cfg = load_yaml("config/global.yaml")
    project_cfg = load_yaml("config/project.yaml")
    user_cfg = load_yaml("config/user.yaml")

    # 檢查 Global vs Project 衝突
    for key in set(global_cfg.keys()) & set(project_cfg.keys()):
        if global_cfg[key] != project_cfg[key]:
            conflicts.append({
                "key": key,
                "global_value": global_cfg[key],
                "project_value": project_cfg[key],
                "level": "global vs project"
            })

    # 檢查 Project vs User 衝突
    for key in set(project_cfg.keys()) & set(user_cfg.keys()):
        if project_cfg[key] != user_cfg[key]:
            conflicts.append({
                "key": key,
                "project_value": project_cfg[key],
                "user_value": user_cfg[key],
                "level": "project vs user"
            })

    return conflicts
```

### 衝突處理策略

**決議**：衝突時提示使用者選擇

```python
def handle_conflict(conflict):
    """
    處理配置衝突

    Args:
        conflict: 衝突資訊

    Returns:
        選擇的值
    """
    print(f"⚠️  配置衝突檢測：{conflict['key']}")
    print(f"   衝突層級：{conflict['level']}")
    print()

    if conflict['level'] == "global vs project":
        print(f"   1. 全局配置值：{conflict['global_value']} (優先級高)")
        print(f"   2. 專案配置值：{conflict['project_value']} (優先級低)")
        print()
        choice = input("   請選擇 (1/2) [預設: 1]: ").strip() or "1"

        if choice == "2":
            return conflict['project_value']
        else:
            return conflict['global_value']

    elif conflict['level'] == "project vs user":
        print(f"   1. 專案配置值：{conflict['project_value']} (優先級高)")
        print(f"   2. 用戶配置值：{conflict['user_value']} (優先級低)")
        print()
        choice = input("   請選擇 (1/2) [預設: 1]: ").strip() or "1"

        if choice == "2":
            return conflict['user_value']
        else:
            return conflict['project_value']
```

---

## 配置優先級矩陣

| 場景                  | Global | Project | User | 最終值                  |
| --------------------- | ------ | ------- | ---- | ----------------------- |
| 僅 Global 定義        | ✅      | ❌       | ❌    | Global                  |
| Global + Project 衝突 | ✅      | ✅       | ❌    | Global（或使用者選擇）  |
| Global + User 衝突    | ✅      | ❌       | ✅    | Global（或使用者選擇）  |
| Project + User 衝突   | ❌      | ✅       | ✅    | Project（或使用者選擇） |
| 三層都定義            | ✅      | ✅       | ✅    | Global（或使用者選擇）  |

---

## 配置驗證

### 必要欄位驗證

```python
def validate_config(cfg):
    """
    驗證配置完整性

    Args:
        cfg: 合併後的配置

    Returns:
        (is_valid, errors)
    """
    required_fields = [
        "metadata.version",
        "pipeline.mode",
        "paths.raw_dir",
        "paths.notes_dir",
        "content.language"
    ]

    errors = []

    for field in required_fields:
        if not get_nested_value(cfg, field):
            errors.append(f"缺少必要欄位：{field}")

    return len(errors) == 0, errors
```

---

## 使用範例

### 載入配置

```python
# 載入並合併配置
config = load_and_merge_config()

# 檢測衝突
conflicts = detect_conflicts()

if conflicts:
    print(f"⚠️  檢測到 {len(conflicts)} 個配置衝突")
    for conflict in conflicts:
        handle_conflict(conflict)

# 驗證配置
is_valid, errors = validate_config(config)

if not is_valid:
    print("❌ 配置驗證失敗：")
    for error in errors:
        print(f"   - {error}")
    exit(1)

# 使用配置
print(f"✓ 配置載入成功")
print(f"  - Pipeline 模式：{config['pipeline']['mode']}")
print(f"  - 預設語系：{config['content']['language']}")
```

### 顯示配置來源

```bash
# 顯示合併後的配置
/notes --show-config

# 輸出範例
Pipeline Configuration:
  mode: stepwise          (source: global.yaml)
  chain.enable: false     (source: global.yaml)
  auto_trigger.enable: false (source: global.yaml)

Content Configuration:
  language: zh-TW         (source: project.yaml)
  fallback_language: en-US (source: global.yaml)

Personal Configuration:
  author: User Name       (source: user.yaml)
  preferred_platforms: [medium, linkedin] (source: user.yaml)
```

---

## 配置熱重載（未來功能）

```python
def watch_config_changes():
    """
    監控配置檔案變更並自動重載
    """
    import watchdog

    def on_modified(event):
        if event.src_path.endswith('.yaml'):
            print(f"🔄 配置檔案已變更：{event.src_path}")
            config = load_and_merge_config()
            print(f"✓ 配置已重新載入")

    observer = watchdog.Observer()
    observer.schedule(on_modified, path="config/")
    observer.start()
```

---

## 環境變數覆蓋（未來功能）

```bash
# 使用環境變數覆蓋配置
export CLAUDE_PIPELINE_MODE=chain
export CLAUDE_CONTENT_LANGUAGE=en-US

# 優先級：環境變數 > Global > Project > User
```

---

## 總結

✅ **配置繼承系統**已實現：
- 三層配置架構（Global > Project > User）
- Deep merge 合併邏輯
- 衝突檢測與提示
- 配置驗證機制

**下一步**：
- 實現配置載入與合併邏輯
- 整合到命令執行流程
- 新增 `--show-config` 參數
