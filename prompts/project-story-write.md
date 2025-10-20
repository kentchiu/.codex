# Project Story Write

**指令**: `/project-story-write ['功能名稱或模組']`
**版本**: v2.0.0
**功能**: 引導式撰寫高質量User Stories，確保符合INVEST原則
**可用工具**: Read, Write, Edit - 建議限制使用這些工具

---
# Project Story Write - [功能名稱或模組]

## 🎯 用途

引導式撰寫高質量的 User Stories，確保符合 INVEST 原則，產生可執行的需求文檔。

## 📝 使用方法

`/project:story-write [功能名稱或模組]`

## 🚨 注意

1. 先做計劃,在我同意前不要進行任何操作,如果有不明白的地方要跟我討論.
2. 務必深思熟慮 (ultrathink)
3. **任務清單**：


## 🔧 執行流程

### 1. Story 框架準備

- [ ] 解析輸入參數：$ARGUMENTS
- [ ] 載入專案上下文（如果有）
- [ ] 準備 Story 模板

### 2. 互動式 Story 撰寫

#### User Story 標準格式

```
As a [user type]
I want to [action/feature]
So that [benefit/value]
```

#### 完整 Story 結構

1. **Story ID**: `[PROJ-XXX]`
2. **標題**: 簡潔描述功能
3. **故事內容**: As a... I want... So that...
4. **驗收條件** (Acceptance Criteria)
5. **技術註記** (Technical Notes)
6. **優先級**: High/Medium/Low
7. **Story Points**: 1, 2, 3, 5, 8, 13
8. **相依性**: 其他 Story IDs

### 3. INVEST 原則檢查

- [ ] **I**ndependent (獨立性)
- [ ] **N**egotiable (可協商)
- [ ] **V**aluable (有價值)
- [ ] **E**stimable (可估算)
- [ ] **S**mall (小而美)
- [ ] **T**estable (可測試)

### 4. Story 類型分類

#### 功能型 Story

```markdown
**ID**: FEAT-001
**標題**: 用戶登入功能
**As a** 註冊用戶
**I want to** 使用 email 和密碼登入系統
**So that** 我可以存取個人化內容

**驗收條件**:

- [ ] [ ] 輸入正確憑證可成功登入
- [ ] [ ] 錯誤憑證顯示適當錯誤訊息
- [ ] [ ] 支援「記住我」功能
- [ ] [ ] 登入後導向個人儀表板

**技術註記**:

- [ ] 使用 JWT token 認證
- [ ] 密碼需 bcrypt 加密
- [ ] 實作 rate limiting 防暴力破解
```

#### 技術型 Story

```markdown
**ID**: TECH-001
**標題**: 實作 CI/CD Pipeline
**As a** 開發團隊
**I want to** 自動化建置和部署流程
**So that** 減少手動錯誤並加快交付速度

**驗收條件**:

- [ ] [ ] Git push 觸發自動建置
- [ ] [ ] 通過所有測試才能部署
- [ ] [ ] 支援多環境部署
- [ ] [ ] 部署失敗自動回滾
```

#### 缺陷修復 Story

```markdown
**ID**: BUG-001
**標題**: 修復購物車計算錯誤
**As a** 購物用戶
**I want to** 看到正確的總金額
**So that** 我可以準確知道需支付金額

**驗收條件**:

- [ ] [ ] 折扣正確套用
- [ ] [ ] 稅金計算準確
- [ ] [ ] 多幣別轉換正確
- [ ] [ ] 小數位處理符合規範
```

### 5. Story 品質檢查清單

- [ ] [ ] 故事是否從用戶視角出發？
- [ ] [ ] 價值主張是否清晰？
- [ ] [ ] 驗收條件是否可測試？
- [ ] [ ] 範圍是否適合一個迭代完成？
- [ ] [ ] 是否有不必要的技術細節？
- [ ] [ ] 相依性是否已標註？

### 6. 批量 Story 生成

支援一次生成多個相關 Stories：

```
Epic: 用戶管理系統
├── Story 1: 用戶註冊
├── Story 2: 用戶登入
├── Story 3: 密碼重設
├── Story 4: 個人資料編輯
└── Story 5: 帳號停用/刪除
```

## 📋 Story 模板庫

### 🔐 認證相關

- [ ] 用戶註冊
- [ ] 用戶登入
- [ ] 密碼管理
- [ ] 多因素認證

### 📊 資料管理

- [ ] CRUD 操作
- [ ] 搜尋過濾
- [ ] 匯入匯出
- [ ] 資料視覺化

### 💳 支付相關

- [ ] 購物車
- [ ] 結帳流程
- [ ] 訂單管理
- [ ] 退款處理

### 🔔 通知系統

- [ ] Email 通知
- [ ] 推播通知
- [ ] 應用內訊息
- [ ] 通知偏好設定

## 💡 最佳實踐

1. **保持簡潔**: 一個 Story 應該在 1-2 週內完成
2. **用戶中心**: 始終從用戶價值出發
3. **可測試性**: 每個驗收條件都應該是可驗證的
4. **避免技術**: Story 描述避免過多技術細節
5. **定期精煉**: Story 應該隨專案進展持續優化

## 📚 相關指令

- [ ] `/project:start` - 先澄清專案想法
- [ ] `/project:story-generate` - 從現有材料生成 Stories
- [ ] `/tdd:red` - 基於 Story 開始寫測試

## 📄 輸出格式

生成的 Stories 將保存為：

- [ ] `user-stories.md` - 完整的 Story 列表

## ⚠️ 注意事項

- [ ] Story 不是技術規格書
- [ ] 避免過度細節，保持彈性
- [ ] 定期與團隊 review Stories
- [ ] 根據實際開發調整和分割 Stories

## 版本歷史
- [ ] v2.0.0 (2025-08-17) 標準化格式，增加 allowed-tools，統一注意事項
- [ ] v1.0.0 (原始日期) 初始版本
