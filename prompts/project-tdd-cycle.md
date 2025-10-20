# Project Tdd Cycle

**指令**: `/project-tdd-cycle ['story-id']`
**版本**: v2.0.0
**功能**: 智能規劃和管理完整 Story 的 TDD 實施流程，支援自動執行模式
**可用工具**: Read, Task - 建議限制使用這些工具

---
# TDD Cycle - Story 完整實施管理

## 🎯 用途

在完成第一個 TDD cycle 後，智能規劃剩餘 Acceptance Criteria 的實施步驟，確保以 TDD 方式完成整個 Story。

## 📝 使用方法

`/project:tdd-cycle [story-id]`

## 🔧 執行流程

### 1. 狀態確認階段

#### 前置條件檢查

- [ ] 確認已使用 `/project:tdd-red`、`/project:tdd-green`、`/project:tdd-refactor` 完成至少一個 cycle
- [ ] 載入當前 Story 內容（從 `user-stories.md` 或指定來源）
- [ ] 分析測試檔案識別已完成的 AC

#### 確認對話範例

```
✅ 確認：您已經完成了第一個 TDD cycle
- [ ] 測試檔案：test_story_001_add_task.py
- [ ] 已實現 AC：支援 `todo add` 指令
- [ ] 測試狀態：全部通過 (3 tests)

剩餘的 AC 如下:

- [ ] AC-02: 支援 todo done 指令
- [ ] AC-03: 支援 todo reject 指令
- [ ] AC-04: 確認狀態成功寫入

是否繼續規劃剩餘的 AC？ [Y/n]
```

### 2. Story 分析與展示

#### 進度視覺化

```markdown
## 📊 Story 實施進度

**STORY-001**: CLI 任務管理工具
進度: ██████░░░░░░░░░░░░░░ 25% (1/4 AC)

### ✅ 已完成

- [ ] AC-01: 支援 `todo add "任務描述"` 指令
  └─ 測試: test_add_command_exists ✓
  └─ 測試: test_add_requires_description ✓
  └─ 測試: test_add_accepts_description ✓

### ⬜ 待完成

- [ ] AC-02: 支援 `todo list` 指令
- [ ] AC-03: 支援 `todo done <id>` 指令
- [ ] AC-04: 資料持久化到本地檔案
```

### 3. 智能規劃生成 (ultrathink)

#### 分析維度

1. **依賴關係分析**
   - [ ] AC 之間的前後順序
   - [ ] 共用元件識別
   - [ ] 技術依賴評估

2. **複雜度評估**
   - [ ] 每個 AC 的實施難度
   - [ ] 預期的 TDD cycle 數量
   - [ ] 可能的技術挑戰

3. **執行順序優化**
   - [ ] 基礎功能優先
   - [ ] 漸進式複雜度
   - [ ] 風險最小化

### 4. 詳細執行計劃

#### 計劃輸出格式

```markdown
## 🚀 TDD 實施計劃

### 整體策略

基於當前進度，建議按以下順序實施剩餘功能：

1. 先完成基礎 CRUD 操作 (list, done)
2. 最後實現資料持久化
3. 每個 AC 採用 3-5 個小步驟的 TDD cycle
4. 執行完一次 cycle, 要更新對應 user story 的 Acceptance Criteria 的完成狀態

### 詳細步驟

#### 📋 AC-02: 支援 `todo list` 指令

**預計 Cycles**: 4 個
**建議順序**:

**Cycle 1**: 基礎指令

- [ ] 🔴 Red: test_list_command_exists()
- [ ] 🟢 Green: 添加 list 指令到 CLI
- [ ] 🔄 Refactor: 整理指令註冊邏輯

**Cycle 2**: 空列表處理

- [ ] 🔴 Red: test_list_shows_empty_message()
- [ ] 🟢 Green: 實現空列表提示
- [ ] 🔄 Refactor: 抽取訊息常數

**Cycle 3**: 顯示任務

- [ ] 🔴 Red: test_list_shows_tasks()
- [ ] 🟢 Green: 實現任務列表顯示
- [ ] 🔄 Refactor: 優化顯示格式

**Cycle 4**: 格式化輸出

- [ ] 🔴 Red: test_list_shows_task_details()
- [ ] 🟢 Green: 添加 ID、狀態等資訊
- [ ] 🔄 Refactor: 建立輸出格式化器

#### ✅ AC-03: 支援 `todo done <id>` 指令

**預計 Cycles**: 3 個
**建議順序**:

**Cycle 1**: 指令結構

- [ ] 🔴 Red: test_done_command_exists()
- [ ] 🟢 Green: 添加 done 指令
- [ ] 🔄 Refactor: 統一指令模式

**Cycle 2**: 標記完成

- [ ] 🔴 Red: test_done_marks_task_completed()
- [ ] 🟢 Green: 實現狀態更新
- [ ] 🔄 Refactor: 抽取狀態管理

**Cycle 3**: 錯誤處理

- [ ] 🔴 Red: test_done_handles_invalid_id()
- [ ] 🟢 Green: 添加錯誤訊息
- [ ] 🔄 Refactor: 統一錯誤處理

#### 💾 AC-04: 資料持久化

**預計 Cycles**: 5 個
**建議順序**:

[詳細步驟省略以保持簡潔]
```

### 5. 執行建議與提醒

#### TDD 節奏提醒

```
記住 TDD 的節奏：
1. 🔴 寫一個失敗的測試
2. 🟢 寫最少的程式碼通過測試
3. 🔄 重構改善設計
4. 🔁 重複直到 AC 完成
```

#### 每個 Cycle 的檢查點

- [ ] [ ] 測試真的失敗了嗎？
- [ ] [ ] 實作是最簡單的嗎？
- [ ] [ ] 所有測試都通過嗎？
- [ ] [ ] 需要重構嗎？

### 6. 確認與執行

#### 使用者確認

```
計劃已生成完成！

📊 統計摘要：
- [ ] 剩餘 AC: 3 個
- [ ] 預計 TDD Cycles: 12 個

選項：
[1] 開始執行計劃（手動模式）
[2] 🚀 自動執行所有 AC（自動模式） ⭐ NEW
[3] 調整執行順序
[4] 查看特定 AC 細節
[5] 取消

請選擇: _
```

### 7. 自動執行模式 🆕

#### 自動模式特色

- [ ] **全自動化**：依序執行每個 AC 的所有 TDD cycles
- [ ] **進度追蹤**：實時顯示當前執行狀態
- [ ] **智能暫停**：遇到測試失敗時自動暫停
- [ ] **自動更新**：完成 cycle 後自動更新 Story 狀態

#### 自動執行流程

```
╔═══════════════════════════════════════════════════════════════════╗
║ 🤖 AUTO MODE ACTIVATED - STORY-001                                ║
║ 將自動執行所有剩餘的 AC                                           ║
║ 按 Ctrl+C 可隨時中斷                                              ║
╚═══════════════════════════════════════════════════════════════════╝

[進度條]
整體進度: ██████░░░░░░░░░░░░░░ 30% (1/3 AC)

🎯 開始處理 AC-02: 支援 `todo list` 指令
```

#### 執行每個 Cycle 時

1. **自動執行 RED**

   ```
   正在執行: /project:tdd-red STORY-001 --ac=2 --cycle=1
   ```

2. **自動執行 GREEN**

   ```
   正在執行: /project:tdd-green test_list_command_exists
   ```

3. **自動執行 REFACTOR**

   ```
   正在執行: /project:tdd-refactor test_story_001
   ```

4. **更新進度**
   ```
   ✅ Cycle 1/4 完成！
   ```

#### 錯誤處理

```
❗ 測試失敗！

執行中斷於：AC-02, Cycle 2
原因：test_list_shows_empty_message 測試失敗

選項：
[1] 查看錯誤詳情
[2] 手動修復後繼續
[3] 跳過此 Cycle
[4] 結束自動模式
```

#### 完成後統計

```
╔═══════════════════════════════════════════════════════════════════╗
║ 🎉 STORY-001 完成！                                               ║
║                                                                   ║
║ 統計：                                                            ║
║ - 總執行時間: 25 分鐘                                             ║
║ - 完成 AC: 3/3                                                    ║
║ - 執行 Cycles: 12/12                                              ║
║ - 測試通過率: 100%                                                ║
║                                                                   ║
║ user-stories.md 已自動更新！                                      ║
╚═══════════════════════════════════════════════════════════════════╝
```

## 💡 使用技巧

### 選擇模式

- [ ] **手動模式**：適合學習 TDD、需要精細控制時
- [ ] **自動模式**：適合熟悉 TDD、快速完成功能時

### 最佳實踐

1. **完成一個 cycle 再看計劃**：避免一次看太多而失焦
2. **保持小步前進**：每個測試都要小而專注
3. **遵循計劃但保持彈性**：遇到問題可以調整
4. **定期檢視進度**：完成幾個 AC 後重新執行查看

### 常見情況處理

- [ ] **發現新需求**：先完成當前 AC，再更新 Story
- [ ] **技術阻礙**：可以調整實施順序
- [ ] **時間不足**：優先完成核心 AC

## 📚 相關指令

- [ ] `/project:story-write` - 查看或更新 Story
- [ ] `/project:tdd-red` - 開始新的測試
- [ ] `/project:tdd-green` - 實作功能
- [ ] `/project:tdd-refactor` - 重構程式碼

## 🚨 注意

1. KISS 不要過度設計
2. 先做計劃,在我同意前不要進行任何操作,如果有不明白的地方要跟我討論
3. 務必深思熟慮 (ultrathink)

## ⚠️ 注意事項

- [ ] 必須先完成至少一個完整的 TDD cycle
- [ ] 手動模式不會自動執行，自動模式會依序執行
- [ ] 計劃是建議性的，可根據實際情況調整
- [ ] 專注於一次一個 AC，避免並行開發
- [ ] 自動模式中可隨時 Ctrl+C 中斷

## 版本歷史

- [ ] v2.0.0 (2025-08-17) 標準化格式，增加 allowed-tools，統一注意事項
- [ ] v1.0.0 (原始日期) 初始版本
