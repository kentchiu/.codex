# Command Check

**指令**: `/command-check ['command-name-pattern']`
**版本**: v1.1.0
**功能**: Claude Code commands 健康檢查 - 支援單一/批量/全域分析
**可用工具**: Read, Bash, Glob, Grep, LS, TodoWrite - 建議限制使用這些工具

---
# 檢查 Commands - PDCA 健康分析

基於 PDCA 專業管理循環，執行 Codex CLI commands 的系統性健康檢查。

## 🚨 注意

**⚠️ 重要 ⚠️ 重要 ⚠️ 重要**

1. KISS 不要過度設計
2. 務必深思熟慮 (ultrathink)
3. 先做計劃,在我同意前不要進行任何操作,如果有不明白的地方要跟我討論, 在計劃完成後,會停止下來要求確認,並且提示已經遵守了上述規範
4. 遵循官網的 Slash Command 規範: https://docs.anthropic.com/en/docs/claude-code/slash-commands

## 🎯 核心功能

健康檢查：

- [ ] ✅ Plan: 檢查策略 (範圍定義、檢查標準、資源分配)
- [ ] ✅ Do: 執行掃描 (批量檢測、資料收集、pattern匹配)
- [ ] ✅ Check: 分析結果 (問題分類、健康報告、趨勢分析)
- [ ] ✅ Act: 改善建議 (優先級排序、行動方案、後續追蹤)

## 🚀 使用方式

```bash
/command:check [name-pattern]
```

支援多種檢查模式：

### 📋 檢查模式

- [ ] 全域檢查：`/command:check` → 檢查所有 commands
- [ ] 單一檢查：`/command:check issue:close` → 檢查特定 command
- [ ] 系列檢查：`/command:check issue:*` → 檢查 issue 命名空間下所有 commands
- [ ] 命名空間檢查：`/command:check deploy:*` → 檢查 deploy 相關所有 commands

### 🎯 萬用字元支援

- [ ] `*` 匹配任意字元
- [ ] `issue:*` 匹配 issue 命名空間下所有 commands
- [ ] `*:start` 匹配所有命名空間下的 start commands

## 🔄 PDCA 執行流程

### Plan - 檢查策略 📋

智能檢查規劃，根據輸入參數制定檢查策略：

1. 範圍定義：

- [ ] 🎯 單一模式 檢查特定 command
- [ ] 📊 批量模式 檢查模式匹配的 commands
- [ ] 🌐 全域模式 檢查所有可用 commands

2. 檢查標準：

- [ ] 📝 結構完整性 YAML front matter 和 Markdown 格式
- [ ] 🔧 功能一致性 usage、examples、allowed-tools、version 對應
- [ ] 💡 品質指標 文檔完整度、邏輯合理性

3. 資源分配：

- [ ] 🚀 並行掃描 同時檢查多個檔案
- [ ] 🔍 深度分析 根據問題嚴重性調整檢查深度

⚠️重要 ⚠️:停止下來要求確認

### Do - 執行掃描 🔨

智能掃描引擎，批量檢測並收集資料：

1. 檔案發現：

- [ ] 使用 Glob 工具快速定位目標檔案
- [ ] 支援萬用字元模式匹配
- [ ] 自動排除無效路徑

2. 結構掃描：

- [ ] YAML front matter 解析
- [ ] Markdown 結構分析
- [ ] 必要欄位存在性檢查

3. 內容分析：

- [ ] 使用 Grep 工具進行模式匹配
- [ ] 檢查常見錯誤 patterns
- [ ] 評估文檔品質指標

### Check - 分析結果 ✅

智能結果分析 → 生成簡潔健康報告：

1. 問題分類：

- [ ] 🔴 嚴重問題 影響功能使用的結構性錯誤
- [ ] 🟡 品質問題 影響體驗的內容不完整
- [ ] 🟢 健康狀態 符合標準的優質 commands

2. 統計分析：

- [ ] 📊 總體健康度百分比
- [ ] 📈 問題分佈統計
- [ ] 🎯 改善優先級排序

### Act - 改善建議 🚀

1. 提供簡潔清單格式的健康報告
2. 按優先級列出具體改善建議
3. 推薦使用 `/command:fix` 修正問題

## 🔍 檢查項目清單

### 📋 結構檢查

- [ ] YAML Front Matter 格式
- [ ] 必要欄位完整性跟順序性 (description, argument-hint, allowed-tools, version)
- [ ] 關注點分離結構 (核心邏輯 vs 規則配置區塊)
- [ ] Markdown 標題階層
- [ ] 檔案路徑規範

### ⚙️ 功能檢查

- [ ] allowed-tools 語法
- [ ] argument-hint 格式
- [ ] 工具權限合理性

### 📝 品質檢查

- [ ] 🚨 注意區塊存在
- [ ] 執行流程清晰度
- [ ] 使用範例充足性
- [ ] 錯誤處理完整性

### 🔄 一致性檢查

- [ ] 版本號格式規範
- [ ] 標籤分類合理性
- [ ] 命名規範遵循
- [ ] 跨檔案重複檢測

## 📊 健康報告格式

### 🎯 全域檢查報告

```
📊 Commands 健康檢查報告

📈 總體狀況:
- [ ] 檢查數量: {total} commands
- [ ] 健康狀態: {healthy}%
- [ ] 發現問題: {issues} 項

🔴 嚴重問題 ({critical}):
- [ ] {namespace}:{command} - {問題描述}
- [ ] {namespace}:{command} - {問題描述}

🟡 品質問題 ({warnings}):
- [ ] {namespace}:{command} - {問題描述}
- [ ] {namespace}:{command} - {問題描述}

🟢 健康 Commands ({healthy_count}):
- [ ] {healthy_list}

💡 改善建議:
1. 優先修正嚴重問題: /command:fix {command}
2. 提升文檔品質: 補充 {missing_items}
3. 統一命名規範: {naming_suggestions}
```

### 🎯 單一檢查報告

```
✅ Command 健康檢查: {name}

📊 檢查結果:
- [ ] 結構完整性: {structure_status}
- [ ] 功能一致性: {function_status}
- [ ] 文檔品質: {quality_status}
- [ ] 整體評分: {overall_score}

🔍 發現問題:
- [ ] {issue1}
- [ ] {issue2}

💡 改善建議:
- [ ] 建議使用: /command:fix {name}
- [ ] 重點改善: {improvement_focus}
```

## ⚠️ 錯誤處理

### 📂 無檔案匹配

```
⚠️ 提醒：未找到匹配的 command 檔案

檢查範圍:
- [ ] 搜尋模式: {pattern}
- [ ] 搜尋路徑: .claude/commands/

建議:
1. 確認 command 名稱正確
2. 檢查檔案是否存在
3. 使用 /command:check 查看所有可用 commands
```

### 🚫 權限問題

```
❌ 錯誤：無法讀取 commands 目錄

解決方案:
1. 確認 .claude/commands/ 目錄存在
2. 檢查目錄讀取權限
3. 重新執行指令
```

## 🎯 使用範例

```bash
# 檢查所有 commands
/command:check

# 檢查特定 command
/command:check issue:close

# 檢查 issue 命名空間
/command:check issue:*

# 檢查 deploy 相關 commands
/command:check deploy:*

# 檢查所有 start commands
/command:check *:start
```

## ✅ 驗收標準

功能性：檢查、支援多種模式、準確問題識別
易用性：簡潔報告、清楚分類、具體改善建議  
可靠性：批量處理、錯誤容錯、一致性結果

---

符合 KISS 原則: 定義範圍 → 批量掃描 → 分析結果 → 簡潔報告。

## 版本歷史

- [ ] v1.1.0 (2025-08-20) 執行計劃加入確認步驟跟Codex CLI 文件
- [ ] v1.0.0 (2025-08-17) 初始版本
