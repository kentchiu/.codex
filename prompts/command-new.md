# Command New

**指令**: `/command-new <command-name> <description>`
**版本**: v1.3.1
**功能**: 建立新的 Claude Code command - 基於 User Story 問卷快速生成最簡可工作版本
**可用工具**: Read, Write, Edit, Bash, Glob, Grep, LS, TodoWrite - 建議限制使用這些工具

---
# 建立新 Command

快速建立最簡可工作的 Codex CLI commands，遵循 KISS 原則。

## 🚨 注意

**⚠️ 重要 ⚠️ 重要 ⚠️ 重要**

1. KISS 不要過度設計
2. 務必深思熟慮 (ultrathink)
3. 先做計劃,在我同意前不要進行任何操作,如果有不明白的地方要跟我討論, 在計劃完成後,會停止下來要求確認,並且提示已經遵守了上述規範
4. 遵循官網的 Slash Command 規範: https://docs.anthropic.com/en/docs/claude-code/slash-commands

## 🎯 核心功能

**PDCA 流程**：

- [ ] **Plan**: 生成 User Story 問卷 → 專案的 `.claude/meta/{path}/{name}.md`
- [ ] **Do**: 生成最簡可工作版本，KISS 原則優先
- [ ] **Check**: 能跑即可，後續用 fix 完善
- [ ] **Act**: 輸出到專案的 `.claude/commands/{path}/{name}.md`

## 📁 路徑說明

**Project Scope** (推薦)：檔案建立在當前專案目錄

### 基本格式

- [ ] 問卷：`.claude/meta/{name}.md`
- [ ] Commands：`.claude/commands/{name}.md`
- [ ] 優點：專案隔離、版本控制友好

### Namespace:Command 格式處理

- [ ] **輸入**：`project:setup`
- [ ] **問卷路徑**：`.claude/meta/project/setup.md`
- [ ] **Command 路徑**：`.claude/commands/project/setup.md`
- [ ] **邏輯**：將 `:` 轉換為 `/` 建立子目錄結構

**注意**：所有檔案都會建立在專案目錄(project scope)而非全域的 `~/.claude/` 目錄(User Scope)

## 🚀 使用方式

```bash
/command:new <command-name> "<description>"
```

**命名格式支援**：

- [ ] 簡單格式：`simple-name` → `.claude/commands/simple-name.md`
- [ ] Namespace格式：`namespace:command` → `.claude/commands/namespace/command.md`

**範例**：

- [ ] `/command:new project:setup "專案初始化"`
- [ ] `/command:new git:sync "Git 同步操作"`

**描述必填**：用於生成對應的 User Story 問卷

**觸發關鍵字**： `Done` 開始生成 command

## 📝 User Story 問卷格式

```markdown
# Command 需求問卷：{name}

> 基於描述：「{user_description}」

## 基本資訊

- [ ] Command 名稱：{name}
- [ ] 簡短描述：{user_description}

## User Stories

Story 1:
As a **\_**
I want **\_**
So that **\_**

Story 2: (可選)
As a **\_**
I want **\_**
So that **\_**

Story 3: (可選)
As a **\_**
I want **\_**
So that **\_**

[可自行新增更多 stories...]

## 技術需求

- [ ] 需要的工具：**\_** (如: Bash, Git, API calls)
- [ ] 主要參數：**\_** (如: 檔案路徑, ID, 選項)
- [ ] 預期輸出：**\_** (如: 檔案, 狀態訊息, 報告)

> 💡可以先讓 AI 進行表單填寫跟Review
```

## 🎯 生成的 Command 結構

```markdown
---
description: 關閉指定的 GitHub issue
argument-hint: [issue-number]
allowed-tools: Bash(gh *), Bash(git *)
version: v1.0.0
---

# 關閉 GitHub Issue

關閉指定的 GitHub issue。

## 🚨 注意

**⚠️ 重要 ⚠️ 重要 ⚠️ 重要**

1. KISS 不要過度設計
2. 務必深思熟慮 (ultrathink)
3. 先做計劃,在我同意前不要進行任何操作,如果有不明白的地方要跟我討論, 在計劃完成後,會停止下來要求確認,並且提示已經遵守了上述規範

## 核心邏輯

### 使用方式

`/issue:close <issue-number>`

### 執行流程

1. 檢查 issue 是否存在
2. 提出計劃
3. ⚠️重要 ⚠️:停止下來要求確認
4. 使用 gh cli 關閉 issue
5. 顯示結果

### 範例

`/issue:close 123`
`/issue:close 456`

## 規則配置（可選）

<!-- 預留規則和配置空間，如不需要可保持空白 -->

## 錯誤處理

錯誤時顯示幫助訊息。

## 版本歷史

- [ ] v1.0.0 (2025-08-17) 初始版本
```

---

**符合 KISS 原則**: 最簡可工作，後續用 fix 完善。

## 版本歷史

- [ ] v1.3.1 (2025-08-26) 修正路徑格式：./claude → .claude (隱藏目錄)
- [ ] v1.3.0 (2025-08-25) 修正 namespace:command 路徑處理邏輯，支援子目錄結構
- [ ] v1.2.0 (2025-08-20) 執行計劃加入確認步驟跟Codex CLI 文件
- [ ] v1.1.0 (2025-08-18) 修正路徑 scope：改為 project scope 而非 user scope
- [ ] v1.0.0 (2025-08-17) 初始版本
