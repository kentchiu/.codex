# D2 Export

**指令**: `/d2-export <d2-file-path> [format]`
**版本**: v1.1.0
**功能**: D2 檔案轉換工具，使用 elk 佈局和手繪風格
**可用工具**: Bash(d2 *), Read, Write, Edit - 建議限制使用這些工具

---
# D2 檔案轉換工具

將 D2 檔案轉換為視覺化圖表，使用 elk 佈局和 sketch 手繪風格。

## 🚨 注意
1. KISS 不要過度設計
2. 快速執行，簡潔高效

## 核心邏輯

### 使用方式
```bash
/d2:export <d2-file-path> [format]
```

**參數說明**：
- [ ] `d2-file-path`：必填，D2 檔案路徑
- [ ] `format`：可選，輸出格式 (svg|png)，預設為 svg

### 執行流程
1. 檢查檔案：驗證 D2 檔案存在且 d2 CLI 可用
2. 執行轉換：使用 elk 佈局 + sketch 主題直接轉換
3. 顯示結果：輸出轉換狀態和檔案位置

### D2 轉換命令格式
```bash
d2 --layout=elk --sketch input.d2 output.svg
d2 --layout=elk --sketch input.d2 output.png
```

### 範例
```bash
/d2:export diagram.d2
/d2:export diagram.d2 png
/d2:export ../docs/architecture.d2 svg
```

## 錯誤處理
- [ ] 基本檢查：檔案存在性、d2 CLI 可用性
- [ ] 轉換失敗：直接顯示 d2 錯誤訊息

---

**符合 KISS 原則**: 最簡可工作版本，專注核心轉換功能。

ARGUMENTS: <d2-file-path> [format]