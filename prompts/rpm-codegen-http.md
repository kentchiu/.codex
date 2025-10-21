---
version: 1.0.0
description: 生成 REST Client 測試文件（.http 格式）供 API 測試使用
argument-hint: TARGET="[service|service/endpoint]"
---
# Rpm Codegen Http

**可用工具**: Read, Grep, MultiEdit, TodoWrite, mcp__gitlab-server__get_file_contents - 建議限制使用這些工具

---
# HTTP 測試文件生成器（REST Client）


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


## 🎯 用途

從 GitLab 上的 API 文檔（web.md）自動生成標準格式的 HTTP 測試文件，支援 REST Client 等工具使用。

## 📝 使用方法

```
/codegen:http              # 互動式選擇服務和 API
/codegen:http <service>    # 顯示服務的所有 API 供選擇
/codegen:http <service>/<endpoint>  # 生成特定端點的 API
```

## 🔧 執行步驟

### 第一階段：環境準備

#### 檢查認證狀態

!`grep -q "TOKEN" doc/http/http-client.env.json 2>/dev/null && echo "✅ TOKEN 已配置" || echo "⚠️  請先執行 /codegen:auth 取得 TOKEN"`

#### 檢查現有 HTTP 文件

!`ls -la doc/http/*.http 2>/dev/null | wc -l | xargs -I {} echo "📁 現有 {} 個 HTTP 測試文件"`

### 第二階段：參數解析與服務選擇

#### 參數處理邏輯

1. **無參數** (`/codegen:http`)
   - [ ] 顯示 6 個可用服務列表
   - [ ] 詢問選擇哪個服務（輸入數字 1-6）
   - [ ] 進入 API 選擇流程

2. **僅服務名** (`/codegen:http hwctrl`)
   - [ ] 直接讀取該服務的 web.md
   - [ ] 列出所有 API 供選擇
   - [ ] 詢問要生成哪些 API

3. **服務/端點** (`/codegen:http hwctrl/node-ml`)
   - [ ] 讀取服務的 web.md
   - [ ] 搜尋匹配端點的 API
   - [ ] 顯示找到的 API 供確認

#### 可用的服務列表

1. **hwctrl** - 硬體控制 API (`app/hwctrl/web.md`)
2. **notify** - 通知 API (`app/notify/web.md`)
3. **sysctrl** - 系統控制 API (`app/sysctrl/web.md`)
4. **auth** - 認證 API (`app/auth/web.md`)
5. **history** - 歷史記錄 API (`app/history/web.md`)
6. **task** - 任務 API (`app/task/web.md`)

### 第三階段：API 端點分析與選擇

1. **讀取選定的 web.md**
   - [ ] 使用 GitLab MCP 讀取文檔內容
   - [ ] 使用正則 `(GET|POST|PATCH|DELETE) http:.*` 提取所有 API

2. **API 篩選邏輯**
   - [ ] 無端點參數：顯示所有 API 供選擇
   - [ ] 有端點參數：篩選匹配的 API
   - [ ] 支援模糊匹配（如 `node-ml` 匹配 `/node-ml` 或 `/nodeML`）

3. **互動式選擇**
   - [ ] 以編號形式列出 API
   - [ ] 支援單選或多選（逗號分隔）
   - [ ] 支援 "all" 選擇所有

4. **生成前確認**

   ```
   ========== 生成預覽 ==========
   目標檔案: doc/http/{service}.http
   操作模式: [新增/附加]

   即將生成的 API:
   1. GET /node-ml - 取得設備 nodeML 資料

   確認生成？(y/n):
   ```

### 第四階段：代碼生成

#### 轉換規則

**端口映射表：**
| 服務 | 端口 | 轉換範例 |
|------|------|----------|
| sysctrl | 16400 | `http://localhost:16400/xxx` → `{{API_URL}}/sysctrl/xxx` |
| auth | 16500 | `http://localhost:16500/xxx` → `{{API_URL}}/auth/xxx` |
| notify | 16600 | `http://localhost:16600/xxx` → `{{API_URL}}/notify/xxx` |
| hwctrl | 16700 | `http://localhost:16700/xxx` → `{{API_URL}}/hwctrl/xxx` |
| history | 16800 | `http://localhost:16800/xxx` → `{{API_URL}}/history/xxx` |

#### 生成規則

1. **檔名處理**
   - [ ] **永遠使用服務名稱作為檔名**（如 hwctrl.http）
   - [ ] **不會生成** `{service}-{endpoint}.http` 格式
   - [ ] 檔案統一儲存在 `doc/http/` 目錄
   - [ ] 生成前顯示目標檔案供確認

2. **內容格式**
   - [ ] 標準 HTTP 格式
   - [ ] 包含 `Authorization: Bearer {{TOKEN}}`
   - [ ] 從文檔提取註釋和參數說明
   - [ ] **只有 URL 路徑中的參數**使用 `@prompt` 標記
   - [ ] **JSON body 中的參數不需要** `@prompt` 標記

3. **重複處理**
   - [ ] 檢查目標檔案是否已有相同 API
   - [ ] 提供覆寫或保留選項

### 第五階段：輸出與驗證

1. **寫入檔案**
   - [ ] 生成符合 REST Client 規範的 HTTP 文件
   - [ ] 保持良好的格式和註釋

2. **顯示結果**
   - [ ] 顯示生成的 API 數量
   - [ ] 提供檔案路徑
   - [ ] 建議後續操作

## ⚠️ 注意事項

- [ ] 需要先執行 `/codegen:auth` 取得有效的 TOKEN
- [ ] 確保 GitLab MCP server 正常連接
- [ ] API 文檔必須符合標準格式

## 📚 相關指令

- [ ] `/codegen:auth` - 取得並更新認證 Token（必須先執行）
- [ ] 完整流程：`/codegen:auth` → `/codegen:http`

## 💡 使用範例

### 範例 1：無參數 - 互動式選擇

```bash
> /codegen:http

請選擇服務 (1-6):
1. hwctrl - 硬體控制
2. notify - 通知
...
選擇: 1

找到 25 個 API:
1. GET /info
2. GET /node-ml
...
選擇 API: 2

[預覽確認...]
目標檔案: doc/http/hwctrl.http
```

### 範例 2：指定服務 - 選擇 API

```bash
> /codegen:http hwctrl

找到 25 個 API:
1. GET /info
2. GET /node-ml
...
選擇 API: 1,2,5

[預覽確認...]
目標檔案: doc/http/hwctrl.http
```

### 範例 3：指定端點 - 直接生成

```bash
> /codegen:http hwctrl/node-ml

找到匹配的 API:
1. GET /node-ml - 取得設備 nodeML 資料

[預覽確認...]
目標檔案: doc/http/hwctrl.http  # 注意：不是 hwctrl-node-ml.http
```

## 📋 生成範例

### 輸入文檔格式

```markdown
### GET http://localhost:16700/info

此 url 主要為回傳程式資訊，除錯記錄等級對應...
```

### 輸出 HTTP 格式

```http
### 取得程式資訊
# 回傳程式資訊，除錯記錄等級
GET {{API_URL}}/hwctrl/info
Authorization: Bearer {{TOKEN}}
```

### 正確的 @prompt 使用範例

#### ✅ URL 參數需要 @prompt

```http
### 設定週邊名稱
# @prompt aliases 週邊別名(infeeds/outlets/temprhs等)
# @prompt id 週邊路數

PATCH {{API_URL}}/hwctrl/peripherals/{{aliases}}/{{id}}
Authorization: Bearer {{TOKEN}}
Content-Type: application/json

{
  "name": "Infeed1"
}
```

#### ❌ JSON body 參數不需要 @prompt

```http
### 設定溫濕度校正值
# 設定週邊的溫濕度校準值

PATCH {{API_URL}}/hwctrl/calbrations/temprh
Authorization: Bearer {{TOKEN}}
Content-Type: application/json

{
  "port": 1,
  "kind": 32,
  "value": 25.1
}
```

## 版本歷史

- [ ] v1.0.0 (2025/10/20) 初始版本
