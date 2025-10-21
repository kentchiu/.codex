---
version: 1.0.0
description: 更新指定 server 的 API Token
argument-hint: TARGET="[server-number|ip]"
---
# Rpm Codegen Auth

**可用工具**: Bash, Edit, Read - 建議限制使用這些工具

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

更新指定 server 的 API Token 到環境設定檔。

## 步驟

1. 確認目標 server：
   - [ ] 如果有提供參數 {{ARGS}}，使用該編號或 IP
   - [ ] 如果沒有參數，預設使用 198 (113.196.183.198)
   - [ ] 如果輸入無效，提示正確選項：
     - [ ] 194: 113.196.183.194
     - [ ] 197: 113.196.183.197
     - [ ] 198: 113.196.183.198

2. 驗證輸入：
   - [ ] 接受編號 (194/197/198) 或完整 IP
   - [ ] 確保 IP 格式正確 (113.196.183.xxx)

3. 讀取當前設定：
   - [ ] 讀取 doc/http/http-client.env.json 確認當前設定
   - [ ] 顯示將要更新的 server

4. 更新 Token：
   使用確定的 IP 執行以下步驟：
   - [ ] **SIT Token**:
     - [ ] 執行: `curl -sX POST http://localhost:3000/api/auth/login -H "Content-Type: application/json" -d '{"username":"admin","password":"admin123"}'`
     - [ ] 提取 accessToken 並更新到 doc/http/http-client.env.json 的 sit.TOKEN
   - [ ] **DEV Token**:
     - [ ] 執行: `curl -sX POST https://{確定的IP}/api/auth/login -H "Content-Type: application/json" -d '{"username":"admin","password":"admin123"}'`
     - [ ] 提取 accessToken 並更新到 doc/http/http-client.env.json 的 dev.TOKEN

5. 顯示結果：
   - [ ] 列出已更新的環境設定
   - [ ] 顯示使用的 server IP
   - [ ] 確認 Token 更新成功

## 注意事項

- [ ] 確保 IP 格式正確 (113.196.183.xxx)
- [ ] 保持 HTTPS 協定不變
- [ ] 更新時保留 JSON 格式和縮排
- [ ] 如果 API 呼叫失敗，顯示錯誤訊息

## 版本歷史

- [ ] v1.0.0 (2025/10/20) 初始版本
