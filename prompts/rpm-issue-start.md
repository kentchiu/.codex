# Rpm Issue Start

**指令**: `/rpm-issue-start issue編號`
**版本**: 2.1.0
**功能**: 開始處理 GitLab issue - 分析並提出執行計劃
**可用工具**: mcp__gitlab-server__get_issue, mcp__gitlab-server__list_issue_discussions, Bash, Read, Write, TodoWrite - 建議限制使用這些工具

---

> ⚠️ **RPM 專案專用**
> 此 command 專為 rpm6/app/doc 專案設計，在其他專案中使用可能無法正常運作。
>
> **依賴項目**:
> - GitLab MCP Server (如適用)
> - 專案結構: `.ai/workbench/`, `.claude/rules/`
> - 專案特定配置和檔案結構
>
> **執行前請檢查**:
> 1. 當前是否在正確的專案目錄中
> 2. 相關依賴是否存在
> 3. 必要的環境變數是否已設定

---


## 🚨 重要：本指令採用「深度思考、先計劃、後執行」模式

此指令會先分析 issue 狀態，使用 **ultrathink 深度思考模式**制定詳細執行計劃，**需要您確認後才會執行任何修改操作**。

## 任務

從 GitLab 獲取 issue #{{ARGS}} 的資訊，分析處理狀態，並提出工作計劃。

## 🚨 注意

**⚠️ 重要 ⚠️ 重要 ⚠️ 重要**

1. KISS 不要過度設計
2. 務必深思熟慮 (ultrathink)
3. 先做計劃,在我同意前不要進行任何操作,如果有不明白的地方要跟我討論, 在計劃完成後,會停止下來要求確認,並且提示已經遵守了上述規範

## 執行流程

### 第一階段：分析與計劃（自動執行）

1. **獲取 Issue 資訊**
   - [ ] 使用 mcp**gitlab-server**get_issue 從 rpm6/app/doc 專案獲取 issue 資訊
   - [ ] 使用 mcp**gitlab-server**list_issue_discussions 獲取所有評論

2. **分析處理歷史**
   - [ ] 檢查是否有最近的 "assigned to @kent" 記錄
   - [ ] 尋找 kent 的完成標記（如 "DONE"、"fixed"、"deploy"）
   - [ ] 如果有完成標記後又重新指派給 kent，表示有新問題需要修復
   - [ ] 收集最後一次指派給 kent 之後的所有評論

3. **檢查程式碼狀態**
   - [ ] 檢查相關分支（使用 git branch -a | grep "issue#{{ARGS}}"）
   - [ ] 查看相關提交記錄
   - [ ] 檢查 .ai/workbench/issue-{{ARGS}}.md 是否存在

4. **UI 設計參考**
   - [ ] 重點參考：功能流程和資訊架構
   - [ ] 注意：實現時應遵循專案的 Element Plus 規範

5. **生成執行計劃 (ultrathink)**

   > 🧠 **IMPORTANT: 現在開始使用 ultrathink 深度思考模式**
   >
   > 請進行深度且全面的分析，考慮所有可能的情況和解決方案。
   > 思考時間不受限制，確保分析徹底且完整。

   根據分析結果，我會使用深度思考模式 (ultrathink) 來制定詳細且全面的執行計劃。

   執行計劃將包含：

   #### 📊 結構化分析報告

   ##### 1. 目前狀況分析 (Current State Analysis)

   分析以下內容：
   - [ ] **Issue 基本資訊**: 標題、狀態、標籤、里程碑
   - [ ] **處理歷史**: 指派記錄、完成標記、討論內容
   - [ ] **程式碼狀態**:
     - [ ] 相關分支狀態（使用 `git branch -a | grep "issue#{{ARGS}}"`）
     - [ ] 提交記錄
     - [ ] 工作檔案 `.ai/workbench/issue-{{ARGS}}.md` 是否存在
   - [ ] **缺少什麼**: 列出目前缺少的元件、功能、文件
   - [ ] **限制條件**: 需要注意的技術限制、依賴關係

   ##### 2. 預期結果 (Desired End State)

   明確描述：
   - [ ] **功能規格**: 完成後應該實現什麼功能
   - [ ] **驗收標準**: 如何判斷 issue 已完成
   - [ ] **驗證方式**: 如何測試和驗證結果

   ##### 3. 關鍵發現 (Key Discoveries)

   **重要**: 每個發現必須包含 `file:line` 格式的引用

   - [ ] **重要發現**: [描述] (`file:line`)
   - [ ] **需要遵循的模式**: [描述] (`file:line`)
   - [ ] **技術限制**: [描述] (`file:line`)
   - [ ] **相依元件**: [描述] (`file:line`)

   範例：
   - [ ] 現有的選單配置使用 boardKind 結構 (`app/types/menu.ts:15`)
   - [ ] API Store 遵循 Pinia 3 模式 (`stores/device.ts:10`)
   - [ ] UI 元件使用 Smart/Dumb 模式 (`app/pages/device/components/DeviceList.vue:1`)

   ##### 4. 不做什麼 (What We're NOT Doing)

   **明確列出不在範圍內的項目，防止範圍蔓延**：

   - [ ] [不在此次處理範圍的功能]
   - [ ] [不需要修改的現有元件]
   - [ ] [暫不處理的優化項目]
   - [ ] [延後處理的相關 issue]

   範例：
   - [ ] 不修改現有的登入流程
   - [ ] 不重構整個選單系統
   - [ ] 不處理效能優化（除非 issue 明確要求）

   #### 🎨 UI 設計參考

   - [ ] **注意事項**：
     - [ ] 遵循專案的 Element Plus 規範 (`.claude/rules/ui-standards.md`)
     - [ ] 參考現有模組的實現模式
     - [ ] 重點關注：
       - [ ] 功能流程設計
       - [ ] 資訊架構
       - [ ] 使用者體驗
   - [ ] **相關圖片資源**: 將使用 /issue:image {{ARGS}} 下載

   #### 🚀 建議執行計劃

   **開發步驟建議**：
   - [ ] 標準開發流程和 codegen 指令鏈：

     ```bash
     # 1. HTTP 測試文件
     /codegen:http

     # 2. 選單配置
     /codegen:menu {boardName}

     # 3. API Store
     /codegen:api

     # 4. UI 頁面和組件
     /codegen:ui {issue}

     # 5. 測試文件
     /codegen:test {file}
     ```

   - [ ] 參考規範檔案列表
   - [ ] 開發順序建議

   **工作檔案處理**：
   - [ ] 建立或更新 .ai/workbench/issue-{{ARGS}}.md
   - [ ] 圖片將透過 /issue:image {{ARGS}} 自動下載
   - [ ] 圖片將嵌入到 Markdown 中以便直接預覽

   **分支策略**：
   - [ ] 新 issue：建立 issue#{{ARGS}} 分支
   - [ ] 已完成但有新問題：建立 issue#{{ARGS}}-fix{N} 分支
   - [ ] 進行中：繼續使用現有分支

   **TODO 項目**：
   - [ ] 預計建立的工作項目清單

   **開發規範參考**：
   - [ ] UI 開發參考 `.claude/rules/ui-standards.md`
   - [ ] API 開發參考 `.claude/rules/api-conventions.md`
   - [ ] i18n 參考 `.claude/rules/i18n-guidelines.md`
   - [ ] 專案結構參考 `.claude/rules/project-structure.md`

### 第二階段：執行計劃確認

> ⏸️ **STOP HERE - 等待用戶確認**
>
> 我已完成分析並制定執行計劃，現在需要您的確認。

**確認事項**：

- [ ] 執行計劃是否符合需求
- [ ] codegen 指令序列是否正確
- [ ] 分支策略是否合適

在分析完成後，我會停下來等待您的確認。您可以：

- [ ] ✅ 確認執行計劃無誤，請我繼續執行
- [ ] 📝 提出修改建議，我會調整計劃
- [ ] ❌ 取消操作

**確認後我將執行**：

1. 建立/更新工作檔案
2. 使用 /issue:image {{ARGS}} 下載相關圖片
3. 將確認的計劃寫入工作檔案
4. 建立對應分支
5. **任務清單**：


### 第三階段：執行環境準備（需確認後執行）

在您確認執行計劃後，我將執行以下操作：

1. **環境準備**
   - [ ] 執行 git checkout master && git pull origin master
   - [ ] 建立或切換到適當的分支

2. **工作檔案處理**
   - [ ] 建立/更新 .ai/workbench/issue-{{ARGS}}.md
   - [ ] 執行 /issue:image {{ARGS}} 自動下載所有圖片到 .ai/workbench/issue-{{ARGS}}/ 目錄
     - [ ] 圖片自動使用 {hash*id}*{filename} 格式命名
     - [ ] 自動檢查圖片完整性，支援重試機制
     - [ ] 只下載圖片檔案（PNG, JPG, JPEG, GIF, BMP, SVG, WEBP）
   - [ ] 在工作檔案中直接嵌入圖片（使用 Markdown 語法 `![描述](./issue-{{ARGS}}/圖片檔名)`）
   - [ ] 記錄 UI 設計重點與問題分析
   - [ ] **將確認的執行計劃（包含 codegen 指令鏈）寫入工作檔案**

3. **任務管理**
   - [ ] **任務清單**：

   - [ ] 設定適當的優先順序
   - [ ] **不會自動開始執行開發工作**

⚠️ **執行邊界**：

- [ ] issue:start 只負責分析、計劃和準備
- [ ] 不會自動執行 codegen 或開發工作
- [ ] 所有 issue 類型都執行相同的準備流程

## 決策邏輯

### 分支策略

- [ ] **新 issue**（無相關分支）→ 建議建立 issue#{{ARGS}} 分支
- [ ] **已完成但重新指派**（有完成標記 + 新指派）→ 建議建立 issue#{{ARGS}}-fix{N} 分支
- [ ] **進行中的工作**（有分支但無完成標記）→ 詢問是否繼續使用現有分支
- [ ] **不確定狀態** → 提供詳細資訊讓您決定

## 工作原則

- [ ] 優先從最近的 "assigned to @kent" 時間點分析問題
- [ ] 識別 kent 的完成標記：DONE、done、fixed、deploy 等
- [ ] 保留並整合現有工作檔案的有用資訊
- [ ] 使用並行處理提高效率（如同時獲取 issue 資訊和檢查分支）
- [ ] 不執行 `git push`, `git commit`, `git rm` 等修改遠端的指令
- [ ] **在生成執行計劃時必須使用 ultrathink 進行深度思考**
- [ ] **在第二階段必須停止等待用戶確認，不可自動執行第三階段**

## 使用範例

```bash
/issue:start 673
```

將會：

1. 分析 issue #673 的完整狀態
2. 提出詳細的執行計劃（包含 codegen 指令鏈）
3. **停下來等待您的確認**
4. 確認後執行環境準備：
   - [ ] 建立/更新工作檔案
   - [ ] 使用 /issue:image {{ARGS}} 自動下載相關圖片
   - [ ] 將執行計劃寫入工作檔案
   - [ ] 建立對應分支
   - [ ] **任務清單**：

5. 準備完成，等待您開始開發工作

## 📸 圖片處理規則

**重要**：在建立工作檔案時，必須：

1. 使用 `/issue:image {{ARGS}}` 下載所有相關圖片
2. 圖片會儲存在 `.ai/workbench/issue-{{ARGS}}/` 目錄
3. **在工作檔案中直接嵌入圖片**，使用以下格式：
   ```markdown
   ![圖片描述](./issue-{{ARGS}}/檔名.png)
   ```
4. 這樣使用者可以在 Markdown 編輯器中直接預覽圖片

範例：

```markdown
### 問題截圖

![型號欄位為空](./issue-744/653a48209a95ab7d27349efccde21be6_image.png)
```

## 版本歷史

- [ ] v2.1.0 (2025-10-14) 加入結構化分析報告格式（目前狀況分析、預期結果、關鍵發現、不做什麼）
- [ ] v2.0.0 (2025-08-17) 加入歷史版本信息
- [ ] v1.0.0 (2024-01-01) 初始版本
