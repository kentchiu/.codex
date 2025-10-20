# Rpm Issue Merge

**指令**: `/rpm-issue-merge ['issue編號']`
**版本**: 2.0.0
**功能**: 合併 issue 分支到 master 並記錄處理歷程
**可用工具**: mcp__gitlab-server__get_issue, Bash, Read, Write - 建議限制使用這些工具

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


## 任務

合併 issue #{{ARGS}} 的分支到 master，並更新工作檔案記錄處理歷程。

## 步驟

1. 使用 mcp**gitlab-server**get_issue 從 rpm6/app/doc 專案獲取 issue 標題
2. 確認當前分支是否為 issue#{{ARGS}}
3. 執行 git checkout master && git pull origin master
4. 執行 git merge --squash issue#{{ARGS}}
5. 執行 git commit -m "rpm6#{{ARGS}} <issue_title>"
6. 獲取 commit hash：執行 git rev-parse HEAD
7. 更新工作檔案 .ai/workbench/issue-{{ARGS}}.md：
   - [ ] 如果檔案存在，使用 Read 讀取現有內容
   - [ ] 新增或更新「## 處理歷程」區塊
   - [ ] 記錄 merge 資訊：
     - [ ] Merge 日期時間
     - [ ] Merge 的 commit hash（前 7 碼）
     - [ ] 分支資訊：issue#{{ARGS}} → master
     - [ ] 從工作檔案中提取主要完成項目（如果有）
   - [ ] 保留原有的需求分析和實作方案內容
8. 提示使用者：
   - [ ] 執行 git push origin master 推送更改
   - [ ] 執行 git branch -d issue#{{ARGS}} 刪除本地分支
   - [ ] 工作檔案已更新，方便日後追蹤

## 注意事項

- [ ] commit message 格式必須是 "rpm6#<issue_no> <issue_title>"
- [ ] 使用 squash merge 保持歷史整潔
- [ ] 工作檔案更新格式範例：

  ```markdown
  ## 處理歷程

  ### YYYY-MM-DD HH:MM 完成並合併

  - [ ] 分支：issue#{{ARGS}} → master
  - [ ] Commit：<hash> "rpm6#{{ARGS}} <title>"
  - [ ] 主要完成項目：
    - [ ] ✓ <從實作方案或 todo 中提取的項目>
  - [ ] 備註：使用 squash merge
  ```

## 版本歷史

- [ ] v1.0.0 (2024-01-01) 初始版本
- [ ] v2.0.0 (2025-08-17) 加入歷史版本信息

