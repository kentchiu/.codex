# Rpm Issue View

**指令**: `/rpm-issue-view issue編號`
**版本**: 2.0.0
**功能**: 檢視 issue 詳細資訊和計劃
**可用工具**: mcp__gitlab-server__get_issue, Read - 建議限制使用這些工具

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
顯示 issue #{{ARGS}} 的詳細資訊。

## 步驟
1. 使用 mcp__gitlab-server__get_issue 從 rpm6/app/doc 專案獲取 issue 詳情
2. 顯示：
   - [ ] Issue 標題和編號
   - [ ] 作者和指派者
   - [ ] 標籤和里程碑
   - [ ] 完整描述
   - [ ] GitLab 連結
3. 如果存在 .ai/workbench/issue-{{ARGS}}.md，使用 Read 工具顯示計劃內容
4. 提示目前分支狀態（是否在對應的 issue 分支上）

## 版本歷史
- [ ] v1.0.0 (2024-01-01) 初始版本
- [ ] v2.0.0 (2025-08-17) 加入歷史版本信息