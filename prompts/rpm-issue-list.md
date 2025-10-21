---
version: 1.0.0
description: 列出所有 issue 分支和未結案 issues
---
# Rpm Issue List

**可用工具**: mcp__gitlab-server__list_issues, Bash - 建議限制使用這些工具

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
顯示本地 issue 分支和 GitLab 上指派給你的未結案 issues。

## 步驟
1. 執行 git branch | grep "issue#" 列出所有本地 issue 分支
2. 使用 mcp__gitlab-server__list_issues 從 rpm6/app/doc 專案獲取：
   - [ ] assignee_username: ["kent"]
   - [ ] state: "opened"
3. 整理並顯示：
   - [ ] 本地 issue 分支列表
   - [ ] GitLab 未結案 issues 列表，按優先級分組：
     - [ ] 優先級 1 的 issues（標籤包含 "1"）
     - [ ] 其他 issues
   - [ ] 每個 issue 顯示：編號、標題、類型標籤（bug/enhancement）
   - [ ] 總計未結案數量和本地分支狀態

## 版本歷史
- [ ] v1.0.0 (2025/10/20) 初始版本
