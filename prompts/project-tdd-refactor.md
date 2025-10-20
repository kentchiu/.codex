# Project Tdd Refactor

**指令**: `/project-tdd-refactor [檔案或模組名稱] [--max-rounds=N]`
**版本**: v2.4.0
**功能**: Continuous Refactoring with Automated Improvement
**可用工具**: Read, Write, Edit, Bash, Glob, Grep, Task - 建議限制使用這些工具

---
# TDD Refactor - 測試驅動開發：持續重構階段

## 🎯 用途

在測試保護下**持續重構**程式碼，自動偵測並消除重複，讓設計自然演進，直到達到品質標準。

## 📝 使用方法

`/project:tdd-refactor [檔案或模組名稱] [--max-rounds=N]`

- [ ] 檔案或模組名稱：要重構的目標
- [ ] --max-rounds：最多執行幾輪重構（預設 10）

## 🚨 注意

1. 先做計劃,在我同意前不要進行任何操作,如果有不明白的地方要跟我討論.
2. 務必深思熟慮 (ultrathink)
3. **任務清單**：


## 📚 官方參考資源

### 重構權威資源

- [ ] **Refactoring Catalog**: https://refactoring.com/catalog/
  - [ ] Martin Fowler 的官方重構技術目錄
  - [ ] 包含所有重構技術的詳細說明和範例
- [ ] **Refactoring Book**: https://martinfowler.com/books/refactoring.html
  - [ ] 《Refactoring: Improving the Design of Existing Code》第二版
  - [ ] 重構領域的經典著作

- [ ] **Code Smells**: https://refactoring.guru/refactoring/smells
  - [ ] 完整的 Bad Smells (代碼異味) 目錄
  - [ ] 每個 Bad Smell 的詳細說明和解決方案

- [ ] **Catalog of Refactorings**: https://refactoring.guru/refactoring/catalog
  - [ ] 視覺化的重構技術說明
  - [ ] 包含 Before/After 程式碼範例

## 🏗️ 核心原則

### 重構三原則

1. **重構是為了消除重複，不是為了設計**
   - [ ] ❌ 不要為了模式而模式
   - [ ] ❌ 不要過度設計
   - [ ] ✅ 專注於移除重複程式碼
   - [ ] ✅ 消除重複後如果出現其他的 Bad Smell，再繼續重構

2. **讓測試驅動設計的演進**
   - [ ] 測試決定介面
   - [ ] 重構只改變實作
   - [ ] 設計自然浮現

3. **小步前進，隨時可以撤回**
   - [ ] 每次只做一個小改變
   - [ ] 每步都要測試通過
   - [ ] 保持可撤回狀態

### KISS (Keep It Simple, Stupid)

- [ ] **簡單優先**：選擇最直接的解決方案
- [ ] **避免過度設計**：不要為未來可能性增加複雜度
- [ ] **清晰勝於聰明**：程式碼應該易於理解

### YAGNI (You Ain't Gonna Need It)

- [ ] **只重構需要的**：不要添加當前不需要的功能
- [ ] **移除死代碼**：刪除未使用的程式碼
- [ ] **專注當下**：解決眼前的問題

### SOLID 原則

- [ ] **S**ingle Responsibility：一個類別只負責一件事
- [ ] **O**pen/Closed：對擴展開放，對修改封閉
- [ ] **L**iskov Substitution：子類別應可替換父類別
- [ ] **I**nterface Segregation：介面應該小而專注
- [ ] **D**ependency Inversion：依賴抽象而非具體

### DRY (Don't Repeat Yourself)

- [ ] **消除重複**：相同邏輯只出現一次
- [ ] **提取共用**：將重複程式碼提取為函數或模組
- [ ] **單一真相來源**：每個知識片段只有一個明確位置

## 🔧 執行流程

### 1. 深度分析階段 (新增)

在開始重構前，進行全面的代碼分析：

#### 檢測 Duplicate Code

- [ ] 比對主模組與子模組的函數
- [ ] 識別名稱相同、實現相同的函數（Duplicate Code）
- [ ] 檢查是否需要 Extract Class 或 Move Method
- [ ] 標記違反 DRY 原則的程式碼

#### 檢測 Dead Code

- [ ] 檢測所有未使用的變數和函數（Dead Code）
- [ ] 分析每個變數/函數的使用情況
- [ ] 標記 Unreachable Code 和 Speculative Generality
- [ ] 準備 Remove Dead Code 計劃

### 2. 重構策略：Extract Class 與 Move Method

✅ **正確的重構流程**：

1. **Extract Class** - 將相關功能提取到新類別
2. **Move Method** - 將方法移到正確的類別
3. **執行測試** - 確認功能正常
4. **Remove Duplicate Code** - 刪除所有重複實現

❌ **常見錯誤（會產生 Duplicate Code）**：

- [ ] 複製程式碼到新模組「以防萬一」
- [ ] 保留原始程式碼等「以後」清理
- [ ] 不徹底的重構導致 Duplicate Code 殘留

### 3. 持續重構循環

```
┌─────────────────────────────────────┐
│  1. 掃描重複 → 2. 自動重構          │
│       ↑              ↓              │
│  4. 評估繼續 ← 3. 測試驗證          │
└─────────────────────────────────────┘
```

會自動重複執行直到：

- [ ] 沒有明顯重複（< 3次）
- [ ] 改善幅度 < 5%
- [ ] 達到最大輪數
- [ ] 測試失敗

### 2. 視覺化輸出

#### 執行開始時顯示

```
📊 Story 進度: ██████░░░░░░░░░░░░░░ 30% (1/3 AC 完成)
🎯 當前 AC 進度: ██████░░░░ 60% (3/5 Cycles 完成)

╔═══════════════════════════════════════════════════════════════════╗
║ 🔄 TDD CONTINUOUS REFACTOR - STORY-[編號]: [Story 標題]           ║
║ AC-[編號]: [AC 描述]                                              ║
║ Cycle [當前/總數]: [Cycle 描述]                                   ║
╚═══════════════════════════════════════════════════════════════════╝

🔍 開始掃描程式碼品質問題...
```

### 3. 重構計劃展示

#### 初始掃描後顯示

```
📋 重構計劃總覽
════════════════════════════════════════════════════
發現以下需要改善的問題：

🔴 高優先級（完全重複）：
  • ticker = yf.Ticker(symbol) → 出現 6 次
  • 相同的錯誤處理邏輯 → 出現 4 次

🟡 中優先級（相似模式）：
  • 類似的驗證邏輯 → 出現 5 次
  • 重複的資料處理 → 出現 3 次

🟢 低優先級（魔術數字）：
  • 數字 2 → 出現 8 次
  • 字串 "error" → 出現 6 次

預計執行 3-5 輪重構，改善程度目標：80% → 20%
════════════════════════════════════════════════════

在你同意後開始自動重構...
```

**!!重要!!: 執行到此處要停止下來讓使用這確認**

### 4. 自動重構執行

#### 每輪重構顯示

```
╭─ Round 1/10 ──────────────────────────────────────╮
│ 🎯 Bad Smell: Long Method                         │
│ 📊 Code Quality: ███████░░░ 70% → 75%             │
│ 🔧 Refactoring: Extract Method                    │
│ 📝 Target: process_data() 方法 (45行)             │
╰───────────────────────────────────────────────────╯

變更差異 (Long Method → Extract Method):
─────────────────────────────────────────────────
原始 process_data() 方法 (45 行) 被拆分為:

+ def validate_input(self, data):
+     """提取: 驗證輸入資料"""
+     if not data:
+         raise ValueError("Data cannot be empty")
+     return True
+
+ def transform_data(self, data):
+     """提取: 轉換資料格式"""
+     return data.strip().lower()
+
+ def save_result(self, result):
+     """提取: 儲存處理結果"""
+     self.database.save(result)

  def process_data(self, data):
- [ ] # 45 lines of mixed responsibilities
+     self.validate_input(data)  # 使用提取的方法
+     transformed = self.transform_data(data)
+     result = self.calculate(transformed)
+     self.save_result(result)
+     return result
─────────────────────────────────────────────────
✅ 測試通過 (97/97)
📊 品質改善: Long Method 3個 → 0個

繼續下一輪 (Large Class)...
```

### 5. Bad Smells 與重構技巧對應表

> 📖 詳細說明請參考：https://refactoring.guru/refactoring/smells

| Bad Smell                  | 描述                   | 重構技巧                                  |
| -------------------------- | ---------------------- | ----------------------------------------- |
| **Long Method**            | 方法超過 30 行         | Extract Function                          |
| **Large Class**            | 類別超過 200 行        | Extract Class, Extract Subclass           |
| **Long Parameter List**    | 參數超過 3-4 個        | Introduce Parameter Object                |
| **Duplicate Code**         | 重複的程式碼           | Extract Function, Extract Class           |
| **Feature Envy**           | 過度使用其他類別       | Move Function, Move Field                 |
| **Data Clumps**            | 總是一起出現的資料     | Extract Class, Introduce Parameter Object |
| **Primitive Obsession**    | 過度使用基本型別       | Replace Primitive with Object             |
| **Switch Statements**      | 複雜的 switch/if-else  | Replace Conditional with Polymorphism     |
| **Lazy Class**             | 類別太小沒價值         | Inline Class                              |
| **Speculative Generality** | 未來可能用到的程式碼   | Remove Dead Code                          |
| **Message Chains**         | A.getB().getC().getD() | Hide Delegate                             |
| **Middle Man**             | 類別只做委託           | Remove Middle Man                         |

### 6. 小步重構原則

#### 重構技巧分類

> 📖 完整目錄：https://refactoring.com/catalog/

**基礎重構 (Most Common)**

1. **Extract Function** - 解決 Long Method
2. **Extract Variable** - 解決 Magic Numbers, Complex Expression
3. **Inline Function** - 解決 Lazy Method
4. **Rename Variable** - 解決 Mysterious Name

**移動功能 (Moving Features)** 5. **Move Function** - 解決 Feature Envy 6. **Move Field** - 解決 Inappropriate Intimacy 7. **Extract Class** - 解決 Large Class

**簡化條件邏輯 (Simplifying Conditionals)** 8. **Decompose Conditional** - 解決 Complex Conditional 9. **Replace Nested Conditional with Guard Clauses** - 解決 Arrow Anti-Pattern 10. **Consolidate Conditional Expression** - 解決 Duplicate Conditionals

每次只做**一種**重構，確保測試通過後才繼續。

### 6. 實際重構範例

#### 範例 1：消除重複 (DRY)

```python
# 重構前：重複的驗證邏輯
def add_task(description):
    if not description:
        raise ValueError("Description cannot be empty")
    if len(description) > 100:
        raise ValueError("Description too long")
    # ...

def update_task(id, description):
    if not description:
        raise ValueError("Description cannot be empty")
    if len(description) > 100:
        raise ValueError("Description too long")
    # ...

# 重構後：遵循 DRY
def validate_description(description):
    if not description:
        raise ValueError("Description cannot be empty")
    if len(description) > 100:
        raise ValueError("Description too long")

def add_task(description):
    validate_description(description)
    # ...

def update_task(id, description):
    validate_description(description)
    # ...
```

#### 範例 2：簡化邏輯 (KISS)

```python
# 重構前：過度複雜
def get_task_status(task):
    if task.completed:
        if task.completed_at:
            if task.completed_at < datetime.now():
                return "completed"
            else:
                return "invalid"
        else:
            return "completed"
    else:
        return "pending"

# 重構後：簡單直接
def get_task_status(task):
    return "completed" if task.completed else "pending"
```

#### 範例 3：單一職責 (SOLID - S)

```python
# 重構前：多重職責
class TaskManager:
    def add_task(self, description):
        # 驗證
        # 儲存到資料庫
        # 發送通知
        # 記錄日誌
        pass

# 重構後：職責分離
class TaskValidator:
    def validate(self, description):
        # 只負責驗證
        pass

class TaskRepository:
    def save(self, task):
        # 只負責儲存
        pass

class TaskManager:
    def __init__(self, validator, repository):
        self.validator = validator
        self.repository = repository

    def add_task(self, description):
        self.validator.validate(description)
        task = Task(description)
        self.repository.save(task)
        return task
```

#### 範例 4：Extract Class 解決 Large Class

```python
# Bad Smell: Large Class (違反 SRP)
# commands.py 原本包含太多職責
class Commands:
    def show_stats(self, data):
        # 統計邏輯不應該在 Commands 類別中
        stats = self.calculate_stats(data)
        self.display_in_picker(stats)
        return stats

    def calculate_stats(self, data):
        # Feature Envy: 這個方法更關心 data 而非 Commands
        # ... 40行統計計算邏輯
        pass

# 另一個檔案也有 Duplicate Code
# stats.py
class Stats:
    def show_stats(self, data):
        # Duplicate Code: 相同的40行實現！
        stats = self.calculate_stats(data)
        self.display_in_picker(stats)
        return stats

# 重構技巧: Extract Class + Move Method + Remove Duplicate Code
# commands.py
class Commands:
    def __init__(self):
        self.stats = Stats()  # 組合優於繼承

    def show_stats(self, data):
        # Move Method: 統計邏輯移到 Stats 類別
        return self.stats.show_stats(data)

# stats.py 保留唯一實現
class Stats:
    def show_stats(self, data):
        # 統計邏輯現在在正確的地方
        stats = self.calculate_stats(data)
        self.display_in_picker(stats)
        return stats
```

### 7. 重構完成總結

#### 完成時顯示

```
╔═══════════════════════════════════════════════════════════════════╗
║ ✅ CONTINUOUS REFACTOR COMPLETE!                                  ║
║                                                                   ║
║ 📊 重構成果總結：                                                 ║
║ • 執行輪數: 4 輪                                                  ║
║ • 重複程度: 80% → 18% (改善 62%)                                  ║
║ • 提取函數: 3 個                                                  ║
║ • 提取常數: 5 個                                                  ║
║ • 簡化邏輯: 2 處                                                  ║
║                                                                   ║
║ 🟢 所有測試通過 (97/97)                                           ║
║ 📝 程式碼品質顯著提升                                             ║
╚═══════════════════════════════════════════════════════════════════╝
```

#### 停止標準

自動停止條件：

- [ ] ✅ 沒有明顯重複（< 3 次）
- [ ] ✅ 函數都在 30 行以內
- [ ] ✅ 改善幅度 < 5%
- [ ] ✅ 達到最大輪數

### 8. Story 狀態自動更新 🆕

#### 自動識別當前 Story 和 AC

```bash
# 從測試檔名識別
test_story_001_add_task.py → STORY-001, AC-01

# 從 git 分支識別
feature/story-001-ac-02 → STORY-001, AC-02

# 從最近執行的 tdd-red/green 指令識別
```

#### 更新 user-stories.md

重構完成後自動執行：

1. **定位 Story 和 AC**
   - [ ] 讀取 user-stories.md
   - [ ] 找到對應的 Story 區塊
   - [ ] 定位當前 AC 的 checkbox

2. **更新 checkbox 狀態**

   ```markdown
   # 更新前

   - [ ] [ ] AC-02: 支援 `todo list` 指令

   # 更新後

   - [ ] [x] AC-02: 支援 `todo list` 指令
   ```

3. **計算完成進度**
   - [ ] 統計該 Story 的總 AC 數
   - [ ] 計算已完成的 AC 數
   - [ ] 顯示進度百分比

#### Story 進度視覺化

```
📊 Story 進度更新
══════════════════════════════════════════════════════

STORY-001: CLI 任務管理工具
整體進度: ██████████░░░░░░░░░░ 50% (2/4 AC 完成)

✅ 已完成的 AC:
  • AC-01: 支援 `todo add` 指令
  • AC-02: 支援 `todo list` 指令 ← 剛完成

⬜ 待完成的 AC:
  • AC-03: 支援 `todo done <id>` 指令
  • AC-04: 資料持久化到本地檔案

══════════════════════════════════════════════════════
```

#### 智能下一步建議

根據 Story 進度提供建議：

```
下一步建議：
→ 繼續下一個 AC：使用 /project:tdd-red STORY-001 --ac=3
→ 查看完整計劃：使用 /project:tdd-cycle STORY-001
→ 提交當前進度：git commit -m "完成 STORY-001 AC-02"
```

#### 完成後顯示

```
╔═══════════════════════════════════════════════════════════════════╗
║ ✅ REFACTOR PHASE COMPLETE!                                       ║
║ Cycle 3/5 完成！程式碼品質已提升                                  ║
║                                                                   ║
║ 📊 Story 狀態已更新:                                              ║
║ • 當前 AC 標記為完成 ✓                                            ║
║ • user-stories.md 已自動更新                                      ║
║                                                                   ║
║ 下一步：繼續下一個 cycle 或開始新的 AC                            ║
╚═══════════════════════════════════════════════════════════════════╝
```

### 9. 執行範例

#### 基本使用

```bash
# 自動持續重構（預設最多 10 輪）
/project:tdd-refactor test_story_001_list.py

# 限制重構輪數
/project:tdd-refactor test_story_001_list.py --max-rounds=5

# 明確指定 Story 和 AC
/project:tdd-refactor test_story_001_list.py --story=STORY-001 --ac=2
```

#### 完整工作流程

```bash
# 1. 寫失敗測試
/project:tdd-red STORY-001 --ac=2

# 2. 實作通過測試
/project:tdd-green test_story_001_list.py

# 3. 重構並更新 Story 狀態（本指令）
/project:tdd-refactor test_story_001_list.py
# → 自動更新 AC-02 為完成
# → 顯示 Story 整體進度
# → 提供下一步建議
```

## 💡 最佳實踐

### 持續重構節奏

1. **自動化流程**：讓工具自動偵測和執行
2. **小步前進**：每輪只做一個改變
3. **測試保護**：每步都驗證測試通過
4. **適時停止**：達到品質標準即可

### 避免的陷阱

- [ ] ❌ 一次改太多
- [ ] ❌ 在重構時加新功能
- [ ] ❌ 為了模式而模式
- [ ] ❌ 破壞測試後繼續重構
- [ ] ❌ 過度重構（追求完美）

### 自動停止條件 - 全面 Bad Smells 檢查

- [ ] **結構品質達標**：
  - [ ] Long Method: 所有方法 ≤ 30 行
  - [ ] Large Class: 所有類別 ≤ 200 行
  - [ ] Long Parameter List: 所有方法參數 ≤ 5 個
  - [ ] Complex Method: 圈複雜度 ≤ 10

- [ ] **重複消除完成**：
  - [ ] Duplicate Code: 重複次數 < 3
  - [ ] Similar Patterns: 相似模式 < 3

- [ ] **表達清晰**：
  - [ ] Magic Numbers: 所有數字都有命名
  - [ ] Dead Code: 無未使用程式碼
  - [ ] Poor Naming: 所有命名都有意義

- [ ] **自動停止**：
  - [ ] 整體改善幅度 < 5%
  - [ ] 連續 2 輪沒有發現新問題
  - [ ] 達到最大輪數 (10 輪)
  - [ ] 測試失敗無法修復

## 📋 核心重構檢查 (KISS原則)

### 5個核心Code Smells

| 問題類型     | 檢查標準     | 簡單修復     |
| ------------ | ------------ | ------------ |
| **重複代碼** | 相同邏輯>3處 | 提取為函數   |
| **過長方法** | 方法>30行    | 拆分為小函數 |
| **魔術數字** | 未命名常數   | 提取為常數   |
| **複雜條件** | 嵌套if>3層   | 簡化邏輯     |
| **死代碼**   | 未使用代碼   | 直接刪除     |

### 簡化重構流程

1. **第1輪** - 消除重複代碼
2. **第2輪** - 拆分長方法
3. **第3輪** - 清理其他問題

### 品質標準

每種 Bad Smell 的消除標準：

- [ ] **Long Method**: 所有方法 ≤ 30 行
- [ ] **Large Class**: 所有類別 ≤ 200 行
- [ ] **Duplicate**: 重複次數 < 3
- [ ] **Magic Numbers**: 所有常數都有命名
- [ ] **Dead Code**: 無未使用程式碼

## 📚 相關指令

- [ ] `/project:tdd-red` - 撰寫失敗測試
- [ ] `/project:tdd-green` - 實作通過測試
- [ ] `/project:tdd-cycle` - 管理完整 Story

## ⚠️ 注意事項

- [ ] **測試安全**：保持測試綠燈是首要原則
- [ ] **全面偵測**：現在會偵測 18+ 種常見 Bad Smells
- [ ] **智能優先**：按 🔴高→🟡中→🟢低 順序處理
- [ ] **循序漸進**：每次只處理一種 Bad Smell，確保品質穩步提升
- [ ] **可視化進度**：每輪都顯示具體 diff 和品質改善
- [ ] **自動停止**：達到品質標準後自動停止，避免過度重構
- [ ] **Story 整合**：狀態更新僅在所有測試通過時執行
- [ ] **格式要求**：確保 user-stories.md 存在且格式正確
- [ ] **完整性**：AC 只有在該 cycle 完全完成時才標記為完成

## 🎯 任務指示

使用持續重構流程處理指定的檔案或模組：

**目標**: `$ARGUMENT`

解析參數：

- [ ] 如果包含 `--max-rounds=N`：設定最大重構輪數為 N
- [ ] 否則使用預設 10 輪

開始執行：

1. 深度分析階段 - 掃描跨檔案重複和未使用代碼
2. 制定重構計劃 - 列出所有發現的問題
3. 等待用戶確認 - 展示計劃並獲得同意
4. 執行重構循環 - 逐步改善代碼品質
5. 完成總結 - 展示改善成果

## 版本歷史

- [ ] v2.4.0 (2025-09-11) 加入官方參考資源連結和延伸學習資源
- [ ] v2.3.0 (2025-09-11) 加入 Bad Smells 對應表，基於 Refactoring.com 分類
- [ ] v2.2.0 (2025-09-11) 使用標準重構術語 (Martin Fowler)
- [ ] v2.1.1 (2025-09-11) 統一範例語言為 Python，提升通用性
- [ ] v2.1.0 (2025-09-11) 加入深度分析階段、實戰案例
- [ ] v2.0.0 (2025-08-17) 標準化格式，增加 allowed-tools，統一注意事項
- [ ] v1.0.0 (原始日期) 初始版本
