---
version: 1.0.0
description: 同步 GitLab boardKind.md 選單配置到本地，支援多種同步模式
argument-hint: TARGET="[board-name|menu-item]"
---
# Rpm Codegen Menu

**可用工具**: mcp__gitlab-server__get_file_contents, Read, MultiEdit, Bash, TodoWrite - 建議限制使用這些工具

---
# 選單配置同步工具 (Menu Sync)


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


## 🎯 核心功能

**必須先顯示執行計劃並獲得確認後才執行同步**

同步 GitLab `boardKind.md` 選單配置到本地檔案：

- [ ] **資料來源**: `rpm6/app/doc` 專案的 `/boardKind.md` 檔案
- [ ] **GitLab 路徑**: `https://gitlab.bellwin.com.tw:11443/rpm6/app/doc/-/blob/master/boardKind.md`
- [ ] **目標檔案**:
  - [ ] `composables/menu-configs/*.ts` - 選單結構檔案
  - [ ] `i18n/zh-TW.json` - 中文語系
  - [ ] `i18n/en-US.json` - 英文語系

## 📝 使用方式

```bash
/codegen:menu              # 同步所有電路板選單
/codegen:menu WP3C         # 同步特定電路板 (board-name)
/codegen:menu 溫濕度        # 同步特定選單項目到所有相關電路板
```

### 參數智能識別

1. **無參數** → 同步所有電路板的所有選單
2. **英文參數** → 識別為 board-name，同步該電路板的所有選單
3. **中文參數** → 識別為選單名稱，同步該選單到所有包含它的電路板

## 🔧 執行流程

### 第一階段：參數解析與配置讀取

```typescript
// 參數類型判斷邏輯
if (!argument) {
  mode = "SYNC_ALL";
} else if (isChineseName(argument)) {
  mode = "SYNC_MENU_ITEM";
  // 搜尋所有包含此選單的電路板
} else {
  mode = "SYNC_BOARD";
  // 轉換為對應檔名: WP3C → wp3c.ts
}
```

**讀取 GitLab 配置**：

- [ ] 使用 `mcp__gitlab-server__get_file_contents`
- [ ] 專案 ID: `91` (rpm6/app/doc)
- [ ] 檔案路徑: `/boardKind.md`
- [ ] 分支: `master`
- [ ] 解析 markdown 表格提取各電路板的選單配置
- [ ] 提取選單的中英文名稱對照

### 第二階段：差異分析與計劃生成

**顯示詳細執行計劃**：

```
========================================
📋 選單同步執行計劃
========================================

🎯 同步模式: [全部同步/電路板同步/選單項目同步]
📍 資料來源: GitLab boardKind.md
🔍 目標參數: [參數值]

將執行以下變更：

📁 composables/menu-configs/wp3c.ts
  ✅ 保持不變: menu.infeed
  ➕ 新增項目: menu.outlet (電源插座 / Power Outlet)
  ➖ 移除項目: menu.deprecated
  ✏️ 更新順序: 調整選單顯示順序

📁 i18n/zh-TW.json (menu 區塊)
  ➕ outlet: "電源插座"
  ✏️ infeed: "電源進線" (原值: "輸入電源")

📁 i18n/en-US.json (menu 區塊)
  ➕ outlet: "Power Outlet"
  ✏️ infeed: "Power Input" (原值: "Input Power")

影響統計：
- [ ] 受影響檔案: 5 個
- [ ] 新增項目: 3 個
- [ ] 更新項目: 2 個
- [ ] 移除項目: 1 個

⚠️ 注意事項：
- [ ] 將移除未定義在 boardKind.md 的選單項目
- [ ] i18n key 值將以 boardKind.md 為準

確認執行以上變更？ (y/n):
```

### 第三階段：執行同步

用戶確認後執行：

1. **備份檢查點**

   ```bash
   git status  # 檢查工作區狀態
   ```

2. **執行同步**
   - [ ] 更新 menu-configs/\*.ts
   - [ ] 更新 i18n 檔案
   - [ ] 保持程式碼格式

3. **驗證結果**
   ```bash
   npm run typecheck  # 類型檢查
   ```

## ⚠️ 重要規則

### 參數識別規則

```typescript
// 中文選單名稱映射
const menuNameMap = {
  電源進線: "menu.infeed",
  三相電源: "menu.infeed3p",
  電源插座: "menu.outlet",
  溫濕度: "menu.thm",
  電子鎖: "menu.elock",
  // ...
};

// 電路板名稱轉換
const boardNameMap = {
  WP3C: "wp3c.ts",
  ELOCK: "elock.ts",
  // 其他轉小寫: "DPP" → "dpp.ts"
};
```

### 選單結構規範

```typescript
// composables/menu-configs/*.ts
export const menuItems: MenuItem[] = [
  {
    labelKey: "menu.infeed", // i18n key
    index: "/monitor/infeed", // 路由路徑
    permissionKey: "monitor/infeed", // 權限 key
    type: "item",
  },
];
```

### i18n 更新規則

1. **保持結構**：不改變 JSON 整體結構
2. **區塊管理**：menu 相關 key 集中在 menu 物件內
3. **順序保持**：新 key 加在區塊末尾
4. **值同步**：以 boardKind.md 為準

## 💡 使用範例

### 範例 1：同步所有選單

```bash
/codegen:menu

# 輸出：
📋 選單同步執行計劃
同步模式: 全部同步
將同步 12 個電路板的選單配置...
```

### 範例 2：同步特定電路板

```bash
/codegen:menu WP3C

# 輸出：
📋 選單同步執行計劃
同步模式: 電路板同步
目標電路板: WP3C → wp3c.ts
```

### 範例 3：同步特定選單項目

```bash
/codegen:menu 溫濕度

# 輸出：
📋 選單同步執行計劃
同步模式: 選單項目同步
目標選單: 溫濕度 (menu.thm)
相關電路板: WP3C, DPP, IENV (共 8 個)
```

## 🔍 錯誤處理

- [ ] GitLab 連線失敗 → 顯示錯誤並中止
- [ ] 檔案衝突 → 顯示衝突詳情，要求手動處理
- [ ] 類型檢查失敗 → 顯示錯誤位置，回滾變更

## 🧪 測試驗證

### 測試案例矩陣

| 測試編號 | 參數輸入 | 預期行為                 | 驗證點                       |
| -------- | -------- | ------------------------ | ---------------------------- |
| T01      | 無參數   | 同步所有電路板           | 所有 menu-configs/\*.ts 更新 |
| T02      | `WP3C`   | 只同步 wp3c.ts           | 只有 wp3c.ts 被修改          |
| T03      | `溫濕度` | 同步 menu.thm 到相關板子 | 所有包含 thm 的檔案更新      |
| T04      | `ELOCK`  | 同步 elock.ts            | 檔名特殊處理正確             |
| T05      | `不存在` | 顯示錯誤訊息             | 不執行任何變更               |

### 驗證步驟

```bash
# 執行前檢查
git status

# 執行同步
/codegen:menu [參數]

# 驗證結果
npm run typecheck
git diff --name-only
```

### 回滾機制

```bash
# 如果執行出錯，快速回滾
git checkout -- composables/menu-configs/
git checkout -- i18n/
```

## 📚 相關指令

- [ ] `/codegen:api` - 生成 API 相關程式碼
- [ ] `/i18n:sync` - 同步語系檔案
- [ ] `/i18n:validate` - 驗證語系完整性

## 🔧 實作邏輯

當執行 `/codegen:menu` 命令時，系統會：

1. 從 GitLab 讀取 `rpm6/app/doc/boardKind.md` (專案 ID: 91)
2. 解析各電路板的選單配置
3. 比對現有 menu-configs/\*.ts 和 i18n 檔案
4. 顯示變更計劃並請求確認
5. 執行變更並驗證類型

### 選單映射表

根據 boardKind.md 內容，建立以下中英文對照映射：

```typescript
const menuTranslations = {
  // 設備監控
  "menu.infeed": { zh: "電源進線", en: "Power Input" },
  "menu.infeed3p": { zh: "三相電源", en: "3-Phase Power" },
  "menu.infeed3pBranch": { zh: "三相電源分支", en: "3-Phase Power Branch" },
  "menu.outlet": { zh: "電源插座", en: "Power Outlet" },
  "menu.outletWithCurrent": {
    zh: "電源插座(含電流)",
    en: "Power Outlet (with Current)",
  },
  "menu.eachOutletCurrent": { zh: "各插座電流", en: "Each Outlet Current" },
  "menu.thm": { zh: "溫濕度", en: "Temperature/Humidity" },
  "menu.dryIn": { zh: "乾接點輸入", en: "Dry Contact Input" },
  "menu.dryOut": { zh: "乾接點輸出", en: "Dry Contact Output" },
  "menu.smoke": { zh: "煙霧", en: "Smoke" },
  "menu.water": { zh: "水位", en: "Water Level" },
  "menu.elock": { zh: "電子鎖", en: "Electronic Lock" },

  // 設備控制
  "menu.schedule": { zh: "排程", en: "Schedule" },
  "menu.protocolMonitor": { zh: "網路偵測", en: "Network Detection" },

  // 設備管理
  "menu.deviceParam": { zh: "設備參數", en: "Device Parameters" },
  "menu.card": { zh: "電子鎖卡號", en: "E-Lock Card Number" },
  "menu.timespan": { zh: "時段", en: "Time Period" },
  "menu.holiday": { zh: "例假日", en: "Holiday" },

  // 權限管理
  "menu.account": { zh: "帳號", en: "Account" },
  "menu.group": { zh: "群組", en: "Group" },
  "menu.elockPermission": { zh: "電子鎖權限", en: "E-Lock Permission" },

  // 工程模式
  "menu.host": { zh: "主機", en: "Host" },
};
```

### 板子對應表

```typescript
const boardMenuMap = {
  DPP: [
    "menu.infeed", // 輸入電源
    "menu.outletWithCurrent", // 電源插座(有電流)
    "menu.thm", // 溫濕度
    "menu.schedule", // 排程
    "menu.protocolMonitor", // 網路偵測
    "menu.deviceParam", // 設備參數
  ],
  WPH3: [
    "menu.infeed3p", // 三相電源
    "menu.infeed3pBranch", // 三相電源分支
    "menu.eachOutletCurrent", // 各路電源插座電流
    "menu.thm", // 溫濕度
    "menu.deviceParam", // 設備參數
  ],
  IENV: [
    "menu.dryIn", // 乾接點輸入
    "menu.dryOut", // 乾接點輸出
    "menu.thm", // 溫濕度
    "menu.smoke", // 煙霧
    "menu.water", // 水位
    "menu.deviceParam", // 設備參數
  ],
  // ... 其他板子配置
};
```

## 版本歷史

- [ ] v1.0.0 (2025/10/20) 初始版本
