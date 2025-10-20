# Project Story Generate

**指令**: `/project-story-generate ['專案初始計劃文件路徑']`
**版本**: v2.0.0
**功能**: 從專案規劃文檔自動生成標準格式的User Stories
**可用工具**: Read, Write - 建議限制使用這些工具

---
# Project Story Generate - [專案初始計劃文件路徑]

## 🎯 用途

從專案規劃文檔自動生成標準格式的 User Stories，產生後停止等待 review，再引導進入 TDD 開發流程。

## 📝 使用方法

`/project:story-generate [專案初始計劃文件路徑]`

專案初始計劃文件路徑如果未指定，預設為 docs/project-plan.md

## 🚨 注意

這個指令很重要, 指令的結果會很大幅度的影響我工作的效率,所以, 務必遵守以下規範:

1. 先做計劃,在我同意前不要進行任何操作,如果有不明白的地方要跟我討論.
2. 務必深思熟慮 (ultrathink)

## ⚠️ 注意事項

- [ ] 只生成單一 `user-stories.md` 檔案，只包含 story 內容
- [ ] Stories 保持簡潔，避免過度設計
- [ ] Story ID 使用 STORY-XXX 格式，方便 TDD 指令引用
- [ ] User Story 三段式必須用 code block 包裹
- [ ] AC 必須使用 **AC-XX** 編號格式並包含子項目
- [ ] 執行完成後停止等待 review，不自動執行後續指令

## 🔧 執行流程

### 1. 讀取專案規劃

- [ ] 解析輸入檔案：$ARGUMENTS
- [ ] 提取核心功能列表
- [ ] 識別用戶角色和需求

### 2. 生成 Stories 策略

#### 功能映射規則

從 project-plan.md 提取：

1. **核心功能（MVP）** → 每個標記 [x] 的功能生成一個 Story
2. **用戶角色** → 從「目標用戶」區塊提取
3. **優先級判定**：
   - [ ] 必要功能 [x] → High Priority
   - [ ] 期望功能 [ ] → Medium/Low Priority
4. **Story Points 估算**：
   - [ ] 簡單功能（CRUD）：2-3 points
   - [ ] 中等複雜度：3-5 points
   - [ ] 高複雜度：5-8 points

### 3. Story 格式模板

```markdown
### STORY-XXX: [功能名稱]
```

As a [用戶角色]
I want to [執行動作]
So that [獲得價值]

```

**Acceptance Criteria:**

- [ ] [ ] **AC-01**: [功能描述]
  - [ ] [具體實作細節1]
  - [ ] [具體實作細節2]
  - [ ] [錯誤處理要求]

- [ ] [ ] **AC-02**: [功能描述]
  - [ ] [具體實作細節1]
  - [ ] [具體實作細節2]
  - [ ] [驗證條件]

- [ ] [ ] **AC-03**: [功能描述]
  - [ ] [具體實作細節1]
  - [ ] [具體實作細節2]
  - [ ] [預期結果]
```

#### 格式說明

1. **Story ID**: STORY-001, STORY-002... （三位數字）
2. **標題格式**: 使用中文動詞+名詞（如：新增任務、查看列表）
3. **User Story 三段式**:
   - [ ] 使用 code block 包裹三段式內容
   - [ ] As a：用戶角色
   - [ ] I want to：具體動作
   - [ ] So that：商業價值
4. **AC 要求**：
   - [ ] 編號格式：**AC-01**, **AC-02** 等
   - [ ] 每個 AC 包含 3 個子項目細節
   - [ ] 使用具體的指令格式或實作要求
   - [ ] 可測試、可驗證
   - [ ] 通常 3-5 個 AC
5. **不包含**：Priority 和 Points（保持簡潔）

### 4. 輸出單一檔案

**檔案名稱**: `user-stories.md`
**檔案位置**: 與輸入檔案相同目錄

**檔案結構**:

```markdown
## User Stories

### STORY-001: [功能1]

[Story 內容]

---

### STORY-002: [功能2]

[Story 內容]

---

### STORY-003: [功能3]

[Story 內容]
```

**注意**：檔案只包含 Stories 內容，不包含其他說明或背景資訊

### 5. Story 生成範例

#### 輸入（project-plan.md）：

```markdown
### 3. 核心功能（MVP）

- [ ] [x] 新增任務
- [ ] [x] 查看任務列表
- [ ] [x] 標記任務完成
- [ ] [ ] 設定優先級
```

#### 輸出（user-stories.md）：

```markdown
## User Stories

### STORY-001: 新增任務
```

As a 開發者
I want to 使用 CLI 指令新增任務
So that 我可以在終端工作時快速記錄待辦事項

```

**Acceptance Criteria:**

- [ ] [ ] **AC-01**: 支援基本新增指令
  - [ ] 使用 `todo add "任務描述"` 格式
  - [ ] 任務描述為必填參數
  - [ ] 參數缺失時顯示使用說明

- [ ] [ ] **AC-02**: 驗證輸入並回饋
  - [ ] 成功新增後顯示確認訊息
  - [ ] 顯示新任務的唯一 ID
  - [ ] 任務描述不可為空字串

- [ ] [ ] **AC-03**: 儲存任務資料
  - [ ] 新任務預設狀態為 TODO
  - [ ] 儲存為 Markdown 格式
  - [ ] 確保資料持久化
```

### 6. 執行後提示與 Review 流程

```
══════════════════════════════════════════════════
✅ User Stories 已生成完成！

📄 檔案位置：user-stories.md
📊 統計：3 個 Stories，12 個 Acceptance Criteria

⏸️  請先 Review 生成的 Stories
   cat user-stories.md

✏️  下一步選項：
→ 開始 TDD 開發：/project:tdd-red STORY-001
→ 修改現有 Story：/project:story-write STORY-001
→ 新增 Story：/project:story-write NEW
══════════════════════════════════════════════════
```

**重要**：指令執行到此停止，等待使用者 review 並選擇下一步動作

## 💡 最佳實踐

1. **KISS 原則**: 保持 Stories 簡單明瞭
2. **可測試性**: AC 必須具體可驗證
3. **單一職責**: 一個 Story 只做一件事
4. **快速迭代**: 先完成 MVP，再逐步優化
5. **Review 優先**: 生成後一定要先 review 再開發
6. **格式一致**: 嚴格遵循 AC 編號和子項目格式

## 📚 相關指令

- [ ] `/project:start` - 專案初始規劃
- [ ] `/project:story-write` - 手動撰寫 Stories
- [ ] `/project:tdd-red` - 基於 Story 開始 TDD

## 版本歷史
- [ ] v2.0.0 (2025-08-17) 標準化格式，增加 allowed-tools，統一注意事項
- [ ] v1.0.0 (原始日期) 初始版本
