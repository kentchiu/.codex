---
version: 1.0.0
description: 部署網站到指定的 server
argument-hint: TARGET="[server-number|ip]"
---
# Deploy

**可用工具**: Bash, Read - 建議限制使用這些工具

---
## 任務

將 Nuxt 應用程式建置並部署到指定的 server。

## 步驟

1. 確認部署目標：

   - [ ] 如果有提供參數 {{ARGS}}，使用該 IP
   - [ ] 如果沒有參數，詢問使用者要部署到哪個 server：
     - [ ] 194: 113.196.183.194
     - [ ] 197: 113.196.183.197
     - [ ] 198: 113.196.183.198

2. 部署前檢查：

   - [ ] 確認當前分支和 git 狀態
   - [ ] 詢問是否要繼續部署

3. 執行部署流程：

   - [ ] 執行 `npm run generate` 建置靜態檔案
   - [ ] SSH SSH, SCP 的密碼是 rpm6@bellwin
   - [ ] 使用 SSH 清除遠端目錄：`ssh -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null -o PreferredAuthentications=password -o PubkeyAuthentication=no root@${IP} 'rm -rf /root/rpm6/www/*'`
   - [ ] 使用 SCP 上傳檔案：`scp -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null -o PreferredAuthentications=password -o PubkeyAuthentication=no -r /home/kent/dev/rpm6/web/.output/public/* root@${IP}:/root/rpm6/www/`

4. 部署完成後：
   - [ ] 顯示部署結果
   - [ ] 提供網站連結：https://${IP}

## 注意事項

- [ ] 確保 SSH 金鑰已設定，可以無密碼登入目標 server
- [ ] 部署會清除目標目錄的所有檔案，請確認 IP 正確
- [ ] 建置過程可能需要幾分鐘，請耐心等待

## 版本歷史
- [ ] v1.0.0 (2025/10/20) 初始版本
