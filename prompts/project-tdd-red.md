---
version: 1.0.0
description: 基於 User Story 撰寫單一失敗的測試案例，使用 BDD 格式開始 TDD 循環
argument-hint: STORY="[story-id]"
---
# Project Tdd Red

**可用工具**: Read, Write, Edit, Bash, Glob, Grep - 建議限制使用這些工具

---
# TDD Red - 測試驅動開發：紅燈階段

## 🎯 用途

基於 User Story 撰寫**單一**失敗的測試案例，使用 BDD 格式定義預期行為，開始 TDD 循環的第一步。

## 📝 使用方法

`/project:tdd-red [Story ID]`

## 🔧 執行流程

### 0. 環境偵測

- [ ] 自動偵測專案語言（package.json → JS/TS, pyproject.toml/requirements.txt → Python）
- [ ] 選擇對應的測試框架模板
- [ ] 可透過參數覆寫：`/project:tdd-red STORY-001 --lang=python`

### 1. Story 分析

- [ ] 載入指定的 Story：$ARGUMENTS
- [ ] Story 檔案位置：`./user-stories.md`
- [ ] 支援格式：Markdown 中的 Story 區塊
- [ ] 識別第一個未完成的 AC
- [ ] 將 AC 轉換為 BDD 格式（Given-When-Then）
- [ ] **一次只處理一個 AC**

### 2. BDD 測試設計

#### BDD 格式轉換範例

**中文範例**：

```
AC: 支援 `todo add "任務描述"` 指令

轉換為 BDD：
Given: 我是一個使用 CLI 的開發者
When: 我執行 `todo add "修復登入問題"` 指令
Then: 系統應該接受這個指令並正常執行
```

**英文範例**：

```
AC: Support `todo list` command

轉換為 BDD：
Given: I am a CLI user
When: I execute `todo list` command
Then: The system should display all tasks
```

### 3. 視覺化輸出

#### 執行開始時顯示

```
╔═══════════════════════════════════════════════════════════════════╗
║ 🔴 TDD RED PHASE - STORY-[編號]: [Story 標題]                     ║
║ AC-[編號]: [AC 描述]                                              ║
║ Cycle [當前/總數]: [Cycle 描述]                                   ║
╚═══════════════════════════════════════════════════════════════════╝

正在撰寫測試：[測試函數名稱]
```

### 4. 測試組織結構

#### Story-Based Test Organization

- [ ] **一個 Story = 一個測試類別 = 一個檔案**
- [ ] **檔案命名**: `tests/test_story_[編號]_[功能名稱].py`
- [ ] **Story 描述放在 class docstring**
- [ ] **每個 AC 用註解分組相關測試**

#### 測試語言支援

**Python (pytest)**：

**中文版本** (當 Story 使用中文描述時)：

```python
class TestStory[編號][功能]:
    """
    STORY-[編號]:
    As a [角色]
    I want to [功能]
    So that [價值/目的]

    Acceptance Criteria:
    1. [AC-01 描述]
    2. [AC-02 描述]
    3. [AC-03 描述]
    """

    # AC-01: [驗收條件描述]
    def test_[具體測試行為1](self):
        """測試描述"""
        # 實作單一測試案例
        pass
```

**英文版本** (當 Story 使用英文描述時)：

```python
class TestStory[Number][Feature]:
    """
    STORY-[Number]:
    As a [Role]
    I want to [Feature]
    So that [Value/Purpose]

    Acceptance Criteria:
    1. [AC-01 Description]
    2. [AC-02 Description]
    3. [AC-03 Description]
    """

    # AC-01: [Acceptance Criteria Description]
    def test_[specific_behavior_1](self):
        """Test description"""
        # Implement single test case
        pass
```

**JavaScript/TypeScript (Jest/Vitest)**：

```javascript
// tests/story-001-add-task.test.js

import { describe, it, expect } from "vitest";
import { app } from "../src/cli";

describe("STORY-001: CLI Task Management Tool", () => {
  /*
   - [ ] As a developer
   - [ ] I want to add tasks using CLI commands
   - [ ] So that I can quickly record todo items
   - [ ] * Acceptance Criteria:
   - [ ] 1. Support `todo add "task description"` command
   - [ ] 2. Show success message after adding
   - [ ] 3. Save task to database
   */

  // AC-01: Support `todo add "task description"` command
  it("should have add command in CLI", () => {
    const result = app.help();
    expect(result).toContain("add");
  });
});
```

**Go (testing)**：

```go
// tests/story_001_add_task_test.go

package tests

import (
    "testing"
    "github.com/stretchr/testify/assert"
)

// TestStory001AddTask - STORY-001: CLI Task Management Tool
// As a developer
// I want to add tasks using CLI commands
// So that I can quickly record todo items
func TestStory001AddTask(t *testing.T) {
    // AC-01: Support `todo add "task description"` command
    t.Run("command should exist in CLI", func(t *testing.T) {
        // Test implementation
        assert.Contains(t, GetCommands(), "add")
    })
}
```

#### AC 拆解為 Unit Test 的粒度

一個 AC 通常需要拆解成多個單元測試：

```
AC: 支援 `todo add "任務描述"` 指令

拆解為：
1. test_add_command_exists() - 指令存在性
2. test_add_requires_description() - 參數驗證
3. test_add_accepts_description() - 接受有效輸入
4. test_add_shows_success() - 成功回饋
```

### 5. 實際執行範例

#### 輸入

```
/project:tdd-red STORY-001
```

#### 視覺化輸出

```
╔═══════════════════════════════════════════════════════════════════╗
║ 🔴 TDD RED PHASE - STORY-001: CLI 任務管理工具                    ║
║ AC-01: 支援 `todo add "任務描述"` 指令                            ║
║ Cycle 1/4: 指令存在性                                             ║
╚═══════════════════════════════════════════════════════════════════╝

正在撰寫測試：test_add_command_exists()
```

#### 測試檔案輸出

**中文 Story 範例**：

```python
# tests/test_story_001_add_task.py

from typer.testing import CliRunner
from todo.cli import app


class TestStory001AddTask:
    """
    STORY-001:
    As a 開發者
    I want to 使用 CLI 指令新增任務
    So that 我可以快速記錄待辦事項並提高工作效率

    Acceptance Criteria:
    1. 支援 `todo add "任務描述"` 指令
    2. 新增成功後顯示確認訊息
    3. 任務應該被儲存到資料庫
    """

    # AC-01: 支援 `todo add "任務描述"` 指令
    def test_add_command_exists(self):
        """指令應該存在於 CLI 中"""
        runner = CliRunner()
        result = runner.invoke(app, ["--help"])
        assert "add" in result.output
```

**英文 Story 範例**：

```python
# tests/test_story_002_list_tasks.py

class TestStory002ListTasks:
    """
    STORY-002:
    As a developer
    I want to list all my tasks
    So that I can see what needs to be done

    Acceptance Criteria:
    1. Support `todo list` command
    2. Display tasks with ID, description, and status
    3. Show message when no tasks exist
    """

    # AC-01: Support `todo list` command
    def test_list_command_exists(self):
        """Command should be available in CLI"""
        runner = CliRunner()
        result = runner.invoke(app, ["--help"])
        assert "list" in result.output
```

### 6. 測試執行命令

#### Python

```bash
pytest tests/test_story_001_add_task.py::TestStory001AddTask::test_add_command_exists -v
```

#### JavaScript/TypeScript

```bash
npm test -- story-001-add-task.test.js -t "should have add command"
vitest run tests/story-001-add-task.test.js
```

#### Go

```bash
go test ./tests -run TestStory001AddTask/command_should_exist
```

### 7. 執行原則

1. **單一測試原則**
   - [ ] 一次只寫一個測試案例
   - [ ] 專注於一個 AC 的最小可測試行為
   - [ ] 避免一次建立多個測試檔案

2. **BDD 格式要求**
   - [ ] 必須包含 Given-When-Then 註釋
   - [ ] 測試描述要對應實際的 Story 和 AC
   - [ ] 程式碼結構要清晰反映 BDD 步驟

3. **測試粒度控制**
   - [ ] 從最簡單的行為開始（如：指令是否存在）
   - [ ] 逐步增加複雜度（下一個測試才測試具體功能）
   - [ ] 保持測試的獨立性

### 8. 檔案命名規範

```
tests/test_story_[編號]_[功能名稱].py
```

例如：

- [ ] `test_story_001_add_task.py` - STORY-001 新增任務
- [ ] `test_story_002_list_tasks.py` - STORY-002 列表顯示
- [ ] `test_story_003_update_status.py` - STORY-003 更新狀態

### 9. 進度追蹤

#### Story 整體進度（顯示在視覺化輸出上方）

```
📊 Story 進度: ██████░░░░░░░░░░░░░░ 30% (1/3 AC 完成)
🎯 當前 AC 進度: ████░░░░░░ 40% (2/5 Cycles 完成)

╔═══════════════════════════════════════════════════════════════════╗
║ 🔴 TDD RED PHASE - STORY-001: CLI 任務管理工具                    ║
║ AC-02: 支援 `todo list` 指令                                      ║
║ Cycle 3/5: 顯示任務列表                                           ║
╚═══════════════════════════════════════════════════════════════════╝
```

## 💡 最佳實踐

1. **Story 組織原則**:
   - [ ] 一個 Story = 一個檔案 = 一個測試類別
   - [ ] Story 資訊放在 class docstring
   - [ ] AC 用註解分組測試方法

2. **測試粒度原則**:
   - [ ] 一個 AC 拆解成多個單元測試
   - [ ] 每個測試方法只測一個具體行為
   - [ ] 從最簡單的測試開始（如指令存在性）

3. **TDD 循環原則**:
   - [ ] 一次只寫一個測試（RED）
   - [ ] 實作最小程式碼通過（GREEN）
   - [ ] 重構後再寫下一個測試（REFACTOR）

## 📚 相關指令

- [ ] `/project:story-write` - 撰寫或查看 User Story
- [ ] `/project:tdd-green` - 實作程式碼通過測試
- [ ] `/project:tdd-refactor` - 重構程式碼
- [ ] `/project:tdd-cycle` - 管理完整 Story 的 TDD 流程

## 🚨 注意

1. KISS 不要過度設計
2. 先做計劃,在我同意前不要進行任何操作,如果有不明白的地方要跟我討論
3. 務必啟動 Deep Think

## ⚠️ 注意事項

- [ ] 一次只專注於一個測試案例
- [ ] 確保測試真的會失敗（Red 階段）
- [ ] 使用 BDD 格式讓測試更易理解
- [ ] 測試檔案保持小而專注
- [ ] Story 檔案需要事先存在

## 版本歷史

- [ ] v1.0.0 (2025/10/20) 初始版本
