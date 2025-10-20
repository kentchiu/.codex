# Rpm Codegen Test

**指令**: `/rpm-codegen-test 檔案路徑或測試類型`
**版本**: 2.0.0
**功能**: 生成測試代碼 (Unit/Integration/E2E)
**可用工具**: Read, Write, MultiEdit, Glob, Grep, TodoWrite - 建議限制使用這些工具

---
# 測試代碼生成器


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

根據現有的代碼自動生成對應的測試文件，支援單元測試、整合測試和端到端測試。

## 📝 使用方法

```bash
/codegen:test [選項]
```

### 選項

- [ ] `--type`: 測試類型 (unit|integration|e2e)，預設為 unit
- [ ] `--file`: 要測試的檔案路徑
- [ ] `--coverage`: 是否包含覆蓋率測試，預設為 true

## 🔧 執行步驟

### 第一階段：分析目標

1. **確定測試範圍**
   - [ ] 單一檔案：詢問使用者要測試的檔案路徑
   - [ ] 多個檔案：讓使用者選擇要測試的模組或功能
   - [ ] 自動偵測：根據最近修改的檔案建議測試目標

2. **檢查現有測試**
   - [ ] 檢查是否已有對應的測試檔案
   - [ ] 如果存在，詢問是否要新增測試案例或覆寫

### 第二階段：測試類型選擇

#### Unit Test (單元測試)

適用於：

- [ ] Composables (`composables/*.ts`)
- [ ] Stores (`stores/*.ts`)
- [ ] Utils/Helpers (`utils/*.ts`)
- [ ] 純函數和類別

#### Integration Test (整合測試)

適用於：

- [ ] API 整合 (`stores/*.ts` with API calls)
- [ ] 多個模組互動
- [ ] 資料流測試

#### E2E Test (端到端測試)

適用於：

- [ ] Pages (`pages/**/*.vue`)
- [ ] 完整使用者流程
- [ ] 表單提交和驗證

### 第三階段：代碼分析

1. **讀取目標檔案**
   - [ ] 解析程式碼結構
   - [ ] 識別可測試的函數和方法
   - [ ] 分析相依性

2. **智能測試案例生成**
   - [ ] 正常流程測試
   - [ ] 邊界條件測試
   - [ ] 錯誤處理測試
   - [ ] 非同步操作測試（如果適用）

### 第四階段：測試生成

#### 生成規則

1. **檔案位置**

   ```
   原始檔案: composables/useExample.ts
   測試檔案: tests/composables/useExample.test.ts
   ```

2. **測試結構**
   - [ ] 遵循 AAA 模式 (Arrange-Act-Assert)
   - [ ] 使用描述性的測試名稱
   - [ ] 包含 JSDoc 註釋

3. **Mock 策略**
   - [ ] 自動識別需要 mock 的相依性
   - [ ] 生成適當的 mock 函數
   - [ ] 處理 Pinia store 的 mock

### 第五階段：驗證與輸出

1. **執行測試驗證**

   ```bash
   npm run test [generated-test-file]
   ```

2. **覆蓋率檢查**（如果啟用）

   ```bash
   npm run test:coverage [tested-file]
   ```

3. **輸出報告**
   - [ ] 顯示生成的測試數量
   - [ ] 提供測試執行結果
   - [ ] 建議後續操作

## 📋 生成範例

### Composable 測試範例

**原始檔案**: `composables/useTimeFormat.ts`

```typescript
export const useTimeFormat = () => {
  const formatDate = (date: Date): string => {
    return date.toLocaleDateString("zh-TW");
  };

  return { formatDate };
};
```

**生成測試**: `tests/composables/useTimeFormat.test.ts`

```typescript
import { describe, it, expect } from "vitest";
import { useTimeFormat } from "~/composables/useTimeFormat";

describe("useTimeFormat", () => {
  it("should format date correctly", () => {
    const { formatDate } = useTimeFormat();
    const testDate = new Date("2023-06-15");

    const result = formatDate(testDate);

    expect(result).toBe("2023/6/15");
  });

  it("should handle invalid date gracefully", () => {
    const { formatDate } = useTimeFormat();
    const invalidDate = new Date("invalid");

    expect(() => formatDate(invalidDate)).not.toThrow();
  });
});
```

### Store 測試範例

**原始檔案**: `stores/userStore.ts`

```typescript
export const useUserStore = defineStore("user", () => {
  const users = ref<User[]>([]);

  const fetchUsers = async () => {
    const data = await $fetch("/api/users");
    users.value = data;
  };

  return { users, fetchUsers };
});
```

**生成測試**: `tests/stores/userStore.test.ts`

```typescript
import { describe, it, expect, beforeEach, vi } from "vitest";
import { setActivePinia, createPinia } from "pinia";
import { useUserStore } from "~/stores/userStore";

describe("useUserStore", () => {
  beforeEach(() => {
    setActivePinia(createPinia());
  });

  it("should initialize with empty users", () => {
    const store = useUserStore();

    expect(store.users).toEqual([]);
  });

  it("should fetch users successfully", async () => {
    const mockUsers = [
      { id: 1, name: "User 1" },
      { id: 2, name: "User 2" },
    ];

    // Mock $fetch
    global.$fetch = vi.fn().mockResolvedValue(mockUsers);

    const store = useUserStore();
    await store.fetchUsers();

    expect(store.users).toEqual(mockUsers);
    expect(global.$fetch).toHaveBeenCalledWith("/api/users");
  });

  it("should handle fetch errors", async () => {
    global.$fetch = vi.fn().mockRejectedValue(new Error("Network error"));

    const store = useUserStore();

    await expect(store.fetchUsers()).rejects.toThrow("Network error");
  });
});
```

## ⚠️ 注意事項

1. **測試檔案不應修改現有測試**（除非使用者明確要求）
2. **遵循專案的測試慣例**
3. **確保生成的測試可以執行**
4. **適當處理非同步操作和 Vue 響應性**

## 🔄 智能功能

1. **自動偵測測試需求**
   - [ ] 分析函數簽名和返回類型
   - [ ] 識別邊界條件
   - [ ] 偵測非同步操作

2. **相依性處理**
   - [ ] 自動 mock 外部 API
   - [ ] 處理 Pinia store
   - [ ] Mock Vue Router 和其他插件

3. **測試優化**
   - [ ] 避免重複的測試設置
   - [ ] 使用適當的測試工具函數
   - [ ] 生成可重用的測試輔助函數

## 📚 相關指令

- [ ] `/codegen:api` - 生成 API 代碼後，使用此命令生成對應測試
- [ ] `/codegen:ui` - 生成 UI 元件後，使用此命令生成元件測試

## 💡 使用範例

### 範例 1：為單一 Composable 生成測試

```bash
/codegen:test --type=unit --file=composables/useAuth.ts
```

### 範例 2：為 Store 生成整合測試

```bash
/codegen:test --type=integration --file=stores/hwctrl.ts
```

### 範例 3：為頁面生成 E2E 測試

```bash
/codegen:test --type=e2e --file=pages/device/index.vue
```

## 🚀 最佳實踐

1. **先生成程式碼，再生成測試**
2. **定期執行測試確保品質**
3. **保持測試簡潔明瞭**
4. **使用有意義的測試描述**
5. **覆蓋關鍵業務邏輯**

## 📖 參考規範

- [ ] `.claude/rules/testing-guide.md` - 測試框架配置和最佳實踐
- [ ] `.claude/rules/api-conventions.md` - API 測試相關規範
- [ ] `.claude/rules/ui-standards.md` - UI 元件測試規範

## 版本歷史

- [ ] v2.0.0 (2025-08-17) 加入歷史版本信息
- [ ] v1.0.0 (2024-01-01) 初始版本

