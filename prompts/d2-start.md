# D2 Start

**指令**: `/d2-start <filename>`
**版本**: 1.2.0
**功能**: 進入 D2 編輯模式，後續對話遵循 CLAUDE.md D2 規範
**可用工具**: Read, Write, Edit, Bash - 建議限制使用這些工具

---
# D2 編輯模式

進入 D2 檔案編輯模式，啟動後對話會自動遵循 CLAUDE.md 中的 D2 規範。

## 🚨 注意

1. 圖標驗證: 如果有異動到 icon，要先用 curl 驗證 icon links 是否有效

## 🚀 使用方式

```bash
/d2:start <filename>
```

**執行後**：
- [ ] 建立/開啟指定的 .d2 檔案
- [ ] 後續對話自動套用 D2 規範
- [ ] 支援自然語言描述轉 D2 語法
- [ ] 自動驗證圖標連結有效性

## 📋 自然語言支援

支援自然語言描述轉換為簡潔 D2 語法：

**✅ 推薦語法**：
- [ ] 節點：`name: "標籤"`
- [ ] 連線：`A -> B: "關係說明"`
- [ ] 分組：`group: { node1; node2 }`

**❌ 避免使用**：
- [ ] Text blocks: `|md`, `|latex`, `|`
- [ ] Legend 語法
- [ ] 複雜嵌套結構

**KISS 原則**：保持圖表簡潔易讀，專注核心邏輯關係。

## 範例

```bash
/d2:start system-arch

# 後續對話
用戶: 繪製兩個資料庫
AI: [生成對應 D2 語法]

用戶: 連接它們
AI: [添加連線關係]

用戶: 完成
AI: [結束編輯模式]
```

## 🎯 Material Symbols 整合

**唯一推薦使用 Iconify Material Symbols**，提供2500+ Google官方設計圖示：

### 基本語法
```d2
node: "節點名稱" {
  icon: https://api.iconify.design/material-symbols/{icon_name}.svg?color={hex}
}
```

### 🔍 圖標驗證 (KISS 原則)

**自動驗證流程**：編輯時遇到圖標，自動用 curl 檢查連結
```bash
# 簡單驗證 - 檢查 HTTP 狀態
curl -I "https://api.iconify.design/material-symbols/database.svg" 2>/dev/null | head -1
```

**驗證結果處理**：
- [ ] ✅ **200 OK**: 圖標有效，直接使用
- [ ] ❌ **404/其他**: 提示用戶檢查圖標名稱拼寫
- [ ] 🔄 **建議替代**: 提供相似的圖標名稱

### 🚨 使用注意事項
- [ ] D2 自動調整 icon 大小，URL 中的 height/width 參數無效
- [ ] 顏色使用 URL 編碼格式 (`#` → `%23`)
- [ ] 推薦使用語義化的 icon 名稱，保持圖表專業性
- [ ] 只使用 Material Symbols，確保圖標風格一致


## 📋 版本歷史

### v1.2.0 (2025-08-17) 添加語法限制指引，禁用 text block 和 legend
### v1.1.0 (2025-08-17) 限定使用 Material Icon
### v1.0.0 (2025-08-17) 基本 D2 編輯模式啟動

