---
version: 1.0.0
description: 切換 API endpoint server
argument-hint: TARGET="[server-number|ip]"
---
# Change Ip

**可用工具**: Read, Edit, MultiEdit - 建議限制使用這些工具

---
## 任務
切換專案的 API endpoint 到指定的 server。

## 步驟

1. 確認目標 server：
   - [ ] 如果有提供參數 {{ARGS}}，使用該編號或 IP
   - [ ] 如果沒有參數，詢問使用者要切換到哪個 server：
     - [ ] 194: 113.196.183.194
     - [ ] 197: 113.196.183.197
     - [ ] 198: 113.196.183.198

2. 驗證輸入：
   - [ ] 接受編號 (194/197/198) 或完整 IP
   - [ ] 如果輸入無效，提示正確選項

3. 讀取當前設定：
   - [ ] 讀取 README.md 確認當前 server
   - [ ] 顯示當前使用的 server

4. 更新檔案：
   使用 MultiEdit 同時更新以下檔案：
   - [ ] README.md: 更新 API_BASE_URL
   - [ ] nuxt.config.ts: 更新 runtimeConfig.public.apiBase
   - [ ] doc/http/http-client.env.json: 更新 dev.host
   - [ ] stores/mqtt.ts: 更新 MQTT_URL

5. 顯示結果：
   - [ ] 列出所有已更新的檔案
   - [ ] 提醒使用者可能需要重啟開發 server

## 注意事項

- [ ] 確保 IP 格式正確 (113.196.183.xxx)
- [ ] 保持 HTTPS 協定不變
- [ ] 更新時保留原始格式和縮排
- [ ] MQTT URL 格式為 wss://{IP}:9001/mqtt

## 版本歷史
- [ ] v1.0.0 (2025/10/20) 初始版本
