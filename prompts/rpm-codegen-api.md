# Rpm Codegen Api

**指令**: `/rpm-codegen-api API文檔或端點選擇`
**版本**: 2.0.0
**功能**: 從 GitLab API 文檔生成 Pinia 3 stores
**可用工具**: Read, Write, MultiEdit, Glob, Grep, mcp__gitlab-server__get_file_contents, TodoWrite - 建議限制使用這些工具

---
# API Codegen 規則


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


## 選擇 codegen 對象

當使用者要進行 http 文件 codegen 前，列出下面的清單讓使用者選擇要使用哪個文件進行 codegen：

1. `.spec/api-doc/app/hwctrl/web.md` - 硬體控制相關 API
2. `.spec/api-doc/app/notify/web.md` - 通知相關 API
3. `.spec/api-doc/app/sysctrl/web.md` - 系統控制相關 API
4. `.spec/api-doc/app/auth/web.md` - 認證相關 API
5. `.spec/api-doc/app/history/web.md` - 歷史記錄相關 API
6. `.spec/api-doc/app/task/web.md` - 任務相關 API

## 執行步驟

1. 使用者選擇對應的 `web.md` 後，使用正則表達式 `(GET|POST|PATCH|DELETE) http:.*` 列出**所有**的 API
2. 讓使用者選擇要進行 codegen 的 API
3. 產生 pinia 3 的 store 到 `stores/{BASE_NAME}.ts`
4. 如果 API response 超過 3 個屬性，產生到 `types/{BASE_NAME}.ts`

## 參考規範

開發時請參考以下規範檔案：

- [ ] **API 開發規範**: `.claude/rules/api-conventions.md` - 包含 Store 設計、型別定義、錯誤處理、Enum 處理等完整規範

## 版本歷史

- [ ] v2.0.0 (2025-08-17) 加入歷史版本信息
- [ ] v1.0.0 (2024-01-01) 初始版本

