---
version: 1.0.0
description: 以目前修改內容做 git commit，遵循 Conventional Commits 規範並添加 emoji
argument-hint: MESSAGE="[message]"
---
# Git Commit

**可用工具**: Bash(git \*) - 建議限制使用這些工具

---

# Git Commit with Conventional Commits

自動分析當前變更並生成符合 Conventional Commits 規範的 commit 訊息，包含適當的 emoji。

## 🚨 注意

1. commit 前要先讓我確認
2. 已設定不包含 Claude attribution，commit message 將保持純淨

## 核心邏輯

### 使用方式

- [ ] `/git:commit` - 自動分析變更並生成 commit 訊息
- [ ] `/git:commit "custom message"` - 使用自定義訊息，但仍會添加適當的 type 和 emoji

### 執行流程

1. **檢查狀態**: 確認有變更需要提交
2. **分析變更**: 檢查修改的檔案類型和內容
3. **決定類型**: 根據變更自動判斷 commit type
4. **生成訊息**: 建立符合 `type(scope): emoji description` 格式的訊息
5. **用戶確認**: 顯示 commit 內容並等待用戶明確同意
6. **執行提交**: 進行 git commit

### Commit Type 自動判斷邏輯

- [ ] **feat** ✨: 新增功能檔案，新的 API endpoint
- [ ] **fix** 🐛: bug 修復，錯誤處理
- [ ] **docs** 📚: README, 文檔, 註解
- [ ] **style** 💄: CSS, UI 樣式, 格式化
- [ ] **refactor** ♻️: 代碼重構，不改變功能
- [ ] **test** ✅: 測試檔案，測試用例
- [ ] **chore** 🔧: 配置檔案，依賴更新，建構工具
- [ ] **perf** ⚡: 效能優化
- [ ] **ci** 👷: CI/CD 相關

### Emoji 對應表

```
✨ feat     🐛 fix      📚 docs     💄 style
♻️ refactor  ✅ test     🔧 chore    ⚡ perf
👷 ci       🚀 deploy   🔒 security 🌐 i18n
```

### 範例

```bash
# 自動分析
/git:commit
# 結果: feat: ✨ add user authentication system

# 自定義訊息
/git:commit "update login flow"
# 結果: feat: ✨ update login flow
```

## 錯誤處理

- [ ] 沒有變更時提示使用者
- [ ] Git 命令失敗時顯示錯誤訊息
- [ ] 無法判斷類型時預設為 `chore`

## 限制

- [ ] 只執行 `git commit`，不處理 branch、push 等操作
- [ ] 不處理 staged/unstaged 狀態，會 commit 所有變更
- [ ] 僅執行唯讀性質的 git 檢查命令

## 執行指示

**⚠️ 重要**: 執行此 command 時，你必須：

1. 先執行 `git status` 和 `git diff` 顯示要提交的變更
2. 分析變更內容並生成適當的 commit message
3. **完整顯示 commit 內容並停止執行**，等待用戶明確同意
4. 獲得用戶同意後才能執行 `git commit`
5. 絕不能跳過確認步驟直接提交

## 版本歷史

- [ ] v1.0.0 (2025/10/20) 初始版本
