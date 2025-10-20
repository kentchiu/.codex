# Command Fix

**指令**: `/command-fix <command-name> <description>`
**版本**: v1.3.0
**功能**: 診斷並修正現有 Claude Code command - 基於 PDCA 問題解決流程
**可用工具**: Read, Write, Edit, Bash, Glob, Grep, LS, TodoWrite - 建議限制使用這些工具

---
# 修正 Command - PDCA 診斷流程

基於 PDCA 專業管理循環，診斷並修正現有 Codex CLI commands 的問題。

## 🚨 注意

**⚠️ 重要 ⚠️ 重要 ⚠️ 重要**

1. KISS 不要過度設計
2. 務必深思熟慮 (ultrathink)
3. 先做計劃,在我同意前不要進行任何操作,如果有不明白的地方要跟我討論, 在計劃完成後,會停止下來要求確認,並且提示已經遵守了上述規範
4. 遵循官網的 Slash Command 規範: https://docs.anthropic.com/en/docs/claude-code/slash-commands

## 🎯 核心功能

**修正現有 command**：

- [ ] ✅ **Plan**: 問題診斷 (語法錯誤、工具衝突、邏輯缺陷)
- [ ] ✅ **Do**: 執行修正 (保持原有功能、提升品質、修復bug)
- [ ] ✅ **Check**: 效果驗證 (語法檢查、功能測試、相容性確認)
- [ ] ✅ **Act**: 持續改善 (直接更新、提供修正報告)

## 🚀 使用方式

```bash
/command:fix <command-name>
```

**必須提供名稱**：指定要修正的 command 名稱

支援 **namespace:command** 格式：

- [ ] `issue:close` → 修正 `.claude/commands/issue/close.md`
- [ ] `deploy:prod` → 修正 `.claude/commands/deploy/prod.md`

### 📋 參數驗證

- [ ] ⚠️ **name 為必填項目**：若未提供名稱，系統將停止執行
- [ ] ✅ 名稱必須對應現有的 command 檔案
- [ ] ✅ 支援 namespace:command 或 simple-name 格式

## 🔄 PDCA 執行流程

### Plan - 問題診斷 🔍

**智能診斷系統**，自動檢測常見問題：

**1. 結構完整性檢查**：

- [ ] 📝 **YAML Front Matter** 格式正確性
- [ ] 🏷️ **必要欄位** description, argument-hint, allowed-tools, version 存在
- [ ] 🔧 **allowed-tools** 語法有效性

**2. 內容品質檢查**：

- [ ] 📖 **文檔結構** 標題階層合理
- [ ] ⚠️ **注意事項** 包含必要的 🚨 注意區塊

**3. 邏輯一致性檢查**：

- [ ] 🔄 **執行流程** 步驟邏輯合理
- [ ] 🛠️ **工具使用** 工具權限與功能匹配

⚠️重要 ⚠️:停止下來要求確認

### Do - 執行修正 🔨

**智能修正引擎**，根據診斷結果自動修正：

**1. 內容改善**：

- [ ] 統一關注點分離結構格式
- [ ] 補充缺失的使用範例
- [ ] 添加必要的注意事項
- [ ] 優化規則配置區塊內容

**2. 邏輯優化**：

- [ ] 重構不合理的執行流程
- [ ] 調整工具權限設定
- [ ] 修正參數提示格式
- [ ] 分離核心邏輯與配置規則

### Check - 效果驗證 ✅

**驗證檢查** → 確保修正品質：

- [ ] ✅ **格式正確**: YAML front matter 和 Markdown 結構
- [ ] ✅ **內容完整**: 所有必要區塊和資訊齊全
- [ ] ✅ **邏輯合理**: 執行流程和工具使用合理

### Act - 持續改善 🚀

1. 直接更新原始檔案
2. 新增版本記錄到版本歷史區塊(一個版本一行)
3. 提供詳細修正報告
4. 建議後續改善方向

## 🎯 修正報告格式

```
✅ Command 修正完成: {name}

📊 診斷結果:
- [ ] 發現問題: {問題數量}
- [ ] 修正項目: {修正數量}
- [ ] 品質提升: {改善說明}
- [ ] 新版本: v{版本號}

🔧 主要修正:
- [ ] {具體修正項目1}
- [ ] {具體修正項目2}
- [ ] {具體修正項目3}

💡 建議改善:
- [ ] {後續改善建議}
```

## ✅ 驗收標準

**功能性**：診斷修正、保持原有功能、提升整體品質
**易用性**：簡單參數、清楚報告、零中斷流程  
**可靠性**：安全修正、不破壞結構、向上相容

---

**符合 KISS 原則**: 診斷問題 → 智能修正 → 驗證效果 → 直接更新。

## 🎯 任務指示

使用 PDCA 流程診斷並修正指定的 Codex CLI command：

**要修正的 command**: `$ARGUMENT`

### Plan - 問題診斷

1. 讀取 command 檔案：`.claude/commands/$ARGUMENT.md` (或 namespace 格式)
2. 檢查 YAML front matter 完整性
3. 驗證必要欄位：description, argument-hint, allowed-tools, version
4. 確認是否正確使用 `$ARGUMENT` 參數
5. 檢查 prompt 邏輯完整性

### Do - 執行修正

1. 修正 YAML 格式錯誤
2. 補充缺失的欄位
3. 加入 `$ARGUMENT` 參數處理
4. 優化 prompt 邏輯和說明

### Check - 效果驗證

1. 確認檔案格式正確
2. 驗證所有必要欄位存在
3. 測試參數處理邏輯

### Act - 完成更新

1. 更新版本號
2. 記錄修正內容到版本歷史
3. 提供修正報告

## 版本歷史

- [ ] v1.3.0 (2025-08-20) 執行計劃加入確認步驟跟Codex CLI 文件
- [ ] v1.2.0 (2025-08-18) 簡化為 prompt 指示，移除複雜 bash 邏輯
- [ ] v1.1.0 (2025-08-18) 加入 $ARGUMENT 參數處理邏輯和完整執行流程
- [ ] v1.0.0 (2025-08-17) 初始版本

