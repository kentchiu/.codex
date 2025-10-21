---
version: 1.0.0
description: 根據 issue 工作檔案生成 Vue 元件
argument-hint: ISSUE=<issue-number>
---
# Rpm Codegen Ui

**可用工具**: Read, Write, MultiEdit, Glob, Grep, mcp__context7__resolve-library-id, mcp__context7__get-library-docs, TodoWrite - 建議限制使用這些工具

---
# UI Codegen 規則


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


## 📌 與通用規範的關係

### 規範優先級

1. 本指令的特殊規則優先於通用規範
2. 未提及的部分遵循 `ui-standards.md`
3. 衝突處理：
   - [ ] **i18n 處理**：本指令使用硬編碼（覆蓋通用規範）
   - [ ] **驗證器使用**：遵循 `ui-standards.md` 的表單驗證規範
   - [ ] **組件模式**：遵循通用規範的 Smart/Dumb Pattern
   - [ ] **Element Plus**：遵循通用規範

### 特殊規則說明

- [ ] **硬編碼優先**：本指令生成的代碼不使用 i18n，後續由 `/i18n:extract` 處理
- [ ] **範例代碼**：本文檔的範例僅適用於 codegen:ui，不代表通用做法
- [ ] **驗證器使用**：遵循 `ui-standards.md` 的表單驗證規範，優先使用 `useIpValidators`

## 🎯 核心原則

1. **深度思考優先**：使用 Deep Think 模式制定詳細計劃
2. **遵循專案既有模式**：參考現有模組實現，而非外部標準
3. **規範優先於視覺設計**：視覺設計僅供參考，實現必須符合專案規範
4. **整合 Issue 工作流程**：從工作檔案自動獲取所需資訊
5. **❌ 完全不使用 i18n**：所有文字使用硬編碼中文，後續由 `/i18n:extract` 和 `/i18n:en` 處理

### ⚠️ 必須遵守的禁忌規範

生成程式碼時**絕對不要**：

- [ ] ❌ 使用 placeholder attribute（見 ui-standards.md:164）
- [ ] ❌ 使用 i18n 相關函數（$t、t()、useI18n）
- [ ] ❌ 使用 ElMessage（應該用 ElNotification）
- [ ] ❌ 使用自創的驗證訊息（見下方驗證訊息規範）

### 🎯 Common 文字使用規範 (源頭優化策略)

**核心原則**：所有文字內容優先使用 `common.*` 中已存在的硬編碼文字，從源頭確保高復用率

#### 📋 Complete Common 文字對照表

##### 基礎驗證 (common.validation.\*)

| 情境     | 使用硬編碼文字 | 對應 common.validation |
| -------- | -------------- | ---------------------- |
| 必填欄位 | `"必填"`       | `required`             |
| 數值必填 | `"數值必填"`   | `numberRequired`       |
| 數字格式 | `"必須為數字"` | `mustBeNumber`         |
| 格式錯誤 | `"格式錯誤"`   | `invalidFormat`        |
| 驗證失敗 | `"驗證失敗"`   | `failed`               |

##### 操作動作 (common.action.\*)

| 情境      | 使用硬編碼文字                   | 對應 common.action      | 使用場景             |
| --------- | -------------------------------- | ----------------------- | -------------------- |
| 按鈕文字  | `"儲存"`                         | `save`                  | 所有保存按鈕         |
| 按鈕文字  | `"取消"`                         | `cancel`                | 所有取消按鈕         |
| 按鈕文字  | `"新增"`                         | `create`                | 所有創建按鈕         |
| 標題/按鈕 | `"編輯"`                         | `edit`                  | 編輯相關             |
| 按鈕文字  | `"刪除"`                         | `delete`                | 刪除按鈕（圖標按鈕） |
| 確認文字  | `"確認刪除"`                     | `confirmDelete`         | 刪除確認按鈕         |
| 確認訊息  | `"您確定要刪除{type}[{name}]？"` | `confirmDeleteTemplate` | 刪除確認對話框       |
| 按鈕文字  | `"測試"`                         | `test`                  | 測試按鈕             |
| 按鈕文字  | `"複製"`                         | `copy`                  | 複製按鈕             |
| 按鈕文字  | `"重新整理"`                     | `refresh`               | 刷新按鈕             |

##### 操作結果 (common.result.\*)

| 情境     | 使用硬編碼文字 | 對應 common.result | 使用場景         |
| -------- | -------------- | ------------------ | ---------------- |
| 成功通知 | `"儲存成功"`   | `saveSuccess`      | 保存成功通知     |
| 失敗通知 | `"儲存失敗"`   | `saveFailed`       | 保存失敗通知     |
| 成功通知 | `"新增成功"`   | `addSuccess`       | 創建成功通知     |
| 失敗通知 | `"新增失敗"`   | `addFailed`        | 創建失敗通知     |
| 成功通知 | `"更新成功"`   | `updateSuccess`    | 更新成功通知     |
| 失敗通知 | `"更新失敗"`   | `updateFailed`     | 更新失敗通知     |
| 成功通知 | `"刪除成功"`   | `deleteSuccess`    | 刪除成功通知     |
| 失敗通知 | `"刪除失敗"`   | `deleteFailed`     | 刪除失敗通知     |
| 測試結果 | `"測試成功"`   | `testSuccess`      | 測試成功通知     |
| 測試結果 | `"測試失敗"`   | `testFailed`       | 測試失敗通知     |
| 複製結果 | `"複製成功"`   | `copySuccess`      | 複製成功通知     |
| 複製結果 | `"複製失敗"`   | `copyFailed`       | 複製失敗通知     |
| 空狀態   | `"無資料"`     | `noData`           | Table empty-text |

##### 錯誤處理 (common.errorCode.\*)

| 情境       | 使用硬編碼文字   | 對應 common.errorCode | 使用場景      |
| ---------- | ---------------- | --------------------- | ------------- |
| 找不到資源 | `"資源不存在"`   | `notFound`            | ID/項目不存在 |
| 重複衝突   | `"設備已存在"`   | `conflict`            | 唯一性衝突    |
| 參數錯誤   | `"請求無效"`     | `badRequest`          | 請求參數錯誤  |
| 通用錯誤   | `"發生未知錯誤"` | `unknown`             | 未知錯誤      |

#### ✅ 源頭優化使用範例

```typescript
// 🎯 驗證規則 - 使用 common.validation 文字 + useIpValidators
import { useIpValidators } from "~/composables/useIpValidators";
const { ipv4Rule } = useIpValidators();

const rules = reactive<FormRules>({
  // 基本必填
  enabled: [
    { required: true, message: "必填", trigger: "change" }, // ← common.validation.required
  ],
  // 數值欄位
  port: [
    { required: true, message: "數值必填", trigger: "blur" }, // ← common.validation.numberRequired
    { validator: validateNumber, message: "必須為數字", trigger: "blur" }, // ← common.validation.mustBeNumber
  ],
  // IP 格式驗證 - 使用標準驗證器
  ip: [
    { required: true, message: "必填", trigger: "blur" }, // ← common.validation.required
    ipv4Rule, // 使用標準驗證規則（內部也是硬編碼）
  ],
});

// 🎯 通知訊息 - 使用 common.result 文字
const handleSave = async () => {
  try {
    if (isNew.value) {
      await store.create(formData.value);
      ElNotification({
        title: "新增成功", // ← common.result.addSuccess  // ← common.result.addSuccess
        type: "success",
      });
    } else {
      await store.update(id, formData.value);
      ElNotification({
        title: "更新成功", // ← common.result.updateSuccess  // ← common.result.updateSuccess
        type: "success",
      });
    }
  } catch (error) {
    ElNotification({
      title: isNew.value ? "新增失敗" : "更新失敗", // ← common.result.addFailed/updateFailed  // ← common.result.addFailed/updateFailed
      type: "error",
    });
  }
};

// 🎯 刪除確認 - 使用 common.action 文字
const openDeleteConfirmDialog = (item) => {
  deleteConfirmDialog.value?.setConfig({
    message: "您確定要刪除防火牆規則[${item.name}]？", // ← 使用 common.action.confirmDeleteTemplate 格式
    onConfirm: () => handleDelete(item.uId),
  });
};

// 🎯 自定義驗證函數 - 使用 useIpValidators
import { useIpValidators } from "~/composables/useIpValidators";
const { isValidIPv4 } = useIpValidators();

const validateIP = (rule: any, value: any, callback: any) => {
  if (!value) {
    callback(new Error("必填")); // ← common.validation.required
    return;
  }
  if (!isValidIPv4(value)) {
    // 使用標準函數
    callback(new Error("格式錯誤")); // ← common.validation.invalidFormat
  } else {
    callback();
  }
};
```

#### ❌ 禁止使用 (會產生多餘 i18n key)

```typescript
// ❌ 錯誤：使用自創文字，增加 i18n:extract 負擔
const rules = reactive<FormRules>({
  enabled: [
    { required: true, message: "請選擇啟用狀態", trigger: "change" }  // 應該用 "必填"
  ],
  ip: [
    { required: true, message: "請輸入 IP 位址", trigger: "blur" },      // 應該用 "必填"
    { validator: validateIP, message: "請輸入有效的 IP 位址", trigger: "blur" }  // 應該用 "格式錯誤"
  ]
});

// ❌ 錯誤：使用自創通知文字
ElNotification({
  title: "保存成功",     // 應該用 "儲存成功"
  title: "數據保存失敗",  // 應該用 "儲存失敗"
  title: "創建完成",     // 應該用 "新增成功"
  title: "操作成功",     // 應該用具體的操作結果
});

// ❌ 錯誤：使用自創按鈕文字
<el-button>保存</el-button>          // 應該用 "儲存"
<el-button>添加</el-button>          // 應該用 "新增"
<el-button>移除</el-button>          // 應該用 "刪除"
<el-button>確認</el-button>          // 應該用 "確定" (已存在於 common.action)

// ❌ 錯誤：使用自創確認文字
message: "確定要刪除這個項目嗎？"      // 應該用 common.action.confirmDeleteTemplate 格式
```

#### 🎯 文字選擇決策樹

**生成程式碼前必須執行**：

```bash
1. 識別文字類型
   ├─ 按鈕文字 → 檢查 common.action.*
   ├─ 通知訊息 → 檢查 common.result.*
   ├─ 驗證訊息 → 檢查 common.validation.*
   └─ 錯誤訊息 → 檢查 common.errorCode.*

2. 尋找匹配
   ├─ 精確匹配：直接使用
   ├─ 語義相近：使用 common 標準文字
   └─ 無匹配：使用通用文字 (如 "必填")

3. 避免自創
   ├─ 不使用："請輸入"、"請選擇"、"請確認"
   ├─ 不使用：包含具體業務的文字
   └─ 不使用：過於詳細的說明文字
```

## 🧠 執行流程

### 執行前立即檢查

⚠️ **必須確保以下步驟都被執行**：

```
[檢查清單]
☐ 已讀取 issue 工作檔案
☐ 已解析路徑參數或 API 路徑
☐ 已應用路徑映射規則
☐ 已讀取並分析 UI 設計圖
☐ 已判斷 UI 架構模式
☐ 已暫停等待用戶確認
☐ 已生成所有必要的組件檔案
```

### 第一階段：深度分析與計劃 (啟動 Deep Think)

> 🧠 **IMPORTANT: 使用 Deep Think 深度思考模式**
>
> 進行全面分析，考慮所有可能的 UI 實現方案。
> 思考時間不受限制，確保分析徹底且完整。

#### 1.1 前置驗證

```bash
# 檢查 Issue 參數
if (!{{ARGS}}) {
  ❌ 錯誤：必須指定 issue 編號
  提示：使用方式 /codegen:ui {issue_number}
}

# 讀取工作檔案
Read: .ai/workbench/issue-{{ARGS}}.md
if (!exists) {
  ❌ 錯誤：找不到 issue #{{ARGS}} 的工作檔案
  提示：請先執行 /issue:start {{ARGS}} 建立工作檔案
}
```

#### 1.2 資訊解析

從工作檔案中提取：

- [ ] **路徑參數**（從執行計劃中的 `/codegen:ui {path}` 提取）
- [ ] 模組名稱（從路徑參數或標題推斷）
- [ ] UI 設計圖路徑（本地圖片路徑）
- [ ] API endpoints（從執行計劃中）
- [ ] 功能需求描述

**路徑解析邏輯**：

```bash
# 從執行計劃中查找 /codegen:ui 指令
if (執行計劃包含 "/codegen:ui {path}") {
  # 解析路徑參數並應用映射規則
  raw_path = extract_path_from_plan

  # 路徑映射規則（優先級從高到低）
  PATH_MAPPINGS = {
    "system-settings/*": "sys/*",        # 系統設定
    "engineering/*": "eng/*",             # 工程設定
    "device-management/*": "device/*",    # 設備管理
    "notification/*": "notify/*",         # 通知管理
    "user-management/*": "user/*"         # 用戶管理
  }

  # 應用映射
  module_path = apply_path_mapping(raw_path, PATH_MAPPINGS)

  # 示例轉換：
  # system-settings/firewall → sys/firewall
  # engineering/firmware → eng/firmware
  # device-management/list → device/list
} else {
  # 從 API endpoints 推斷
  api_path = extract_api_path()  # 例如：sysctrl/firewall

  # API 到路徑的映射
  API_TO_PATH = {
    "sysctrl/*": "sys/*",
    "hwctrl/*": "device/*",
    "notify/*": "notify/*",
    "user/*": "user/*"
  }

  module_path = apply_api_mapping(api_path, API_TO_PATH)
}

# 最終路徑格式：pages/{mapped_path}/
 final_path = "pages/" + module_path + "/"
```

驗證前置條件：

- [ ] 確認 UI 設計圖存在：`Read: {extracted_image_path}`
- [ ] 確認 API Store 存在：`Read: stores/{module}.ts`
- [ ] 確認類型定義存在：`Read: types/{module}.ts`

#### 1.3 UI 類型判斷

基於 UI 設計圖判斷架構模式：

```bash
# 讀取並顯示 UI 設計圖
Read: {extracted_image_paths}

# 分析 UI 圖片內容
if (圖片包含列表/表格 + 編輯表單) {
  → Table-based Flow (CRUD 模式)
  → 檔案結構：index.vue + [id].vue + components/
} else if (只有單一表單/設定頁面) {
  → Single-page Form (設定模式)
  → 檔案結構：單一 {feature}.vue
} else {
  → 無法判斷，詢問用戶選擇
}
```

##### Table-based Flow (CRUD 模式)

- [ ] **特徵**：有列表頁面 + 編輯/新增頁面
- [ ] **結構**：
  ```
  pages/{module}/
  ├── index.vue        # 列表頁面
  ├── [id].vue        # 編輯頁面
  └── components/     # 共用組件
  ```
- [ ] **參考模組**：pages/user/, pages/notify/, pages/eng/syslog/

##### Single-page Form (設定模式)

- [ ] **特徵**：只有單一設定/配置頁面
- [ ] **結構**：
  ```
  pages/{module}/
  └── {feature}.vue   # 單一頁面
  ```
- [ ] **參考模組**：pages/eng/firmware.vue, pages/eng/ldap.vue

#### 1.4 深度思考分析

使用 Deep Think 分析選定模式的實現細節：

1. **架構實現細節**
   - [ ] 根據選定模式規劃檔案
   - [ ] 評估組件拆分策略
   - [ ] 確定資料流向

2. **風險評估**
   - [ ] 識別可能的技術挑戰
   - [ ] 檢查現有檔案衝突
   - [ ] 評估實現複雜度

### 第二階段：生成執行計劃

#### 2.1 架構決策說明

```markdown
## 🏗️ 架構決策

### UI 類型判斷結果

根據 UI 設計圖分析：
[顯示分析的圖片路徑和判斷理由]

選擇：[Table-based Flow / Single-page Form]

### 生成路徑確認

根據分析，檔案將生成到：
**pages/{解析的路徑}/**

例如：

- [ ] 從執行計劃解析：system-settings/firewall → pages/system-settings/firewall/
- [ ] 從 API 推斷：firewall → pages/firewall/
- [ ] 常見路徑：
  - [ ] pages/sys/{feature}/ - 系統設定類
  - [ ] pages/eng/{feature}/ - 工程設定類
  - [ ] pages/device/{feature}/ - 設備管理類
  - [ ] pages/{feature}/ - 獨立功能模組

### 生成檔案清單

[根據選擇的模式列出將生成的檔案]

### 參考模組

[列出對應模式的參考模組]
```

### 第三階段：確認與執行

> ⏸️ **STOP HERE - 等待用戶確認**
>
> 我已完成 UI 類型分析並制定執行計劃。
>
> **判斷結果**：[Table-based Flow / Single-page Form]
>
> **生成路徑**：`pages/{解析的路徑}/`
>
> 路徑解析過程：
>
> - 原始參數：{顯示傳入的參數}
> - 映射規則：{顯示應用的規則}
> - 最終路徑：{顯示最終路徑}
>
> **生成檔案**：
>
> ```
> pages/{module}/
> ├── index.vue              # 列表頁面
> ├── [id].vue              # 編輯頁面
> └── components/
>     ├── {Module}Table.vue  # 表格組件
>     ├── {Module}Form.vue   # 表單組件
>     └── index.ts          # 組件導出
> ```
>
> 請確認：
>
> 1. **路徑是否正確？** 當前：`pages/{解析的路徑}/`
>    - 輸入新路徑可直接修改（例如：`sys/firewall`）
>    - 輸入 `y` 確認當前路徑
> 2. **架構模式是否正確？** 當前：[Table-based Flow / Single-page Form]
>    - 輸入 `table` 或 `single` 可切換模式
>    - 輸入 `y` 確認當前模式
>
> **重要**：此步驟必須等待用戶確認後才能繼續

確認後將執行：

#### Table-based Flow 生成模板

##### 檔案結構

```
pages/{module}/
├── index.vue              # 列表頁面 (Smart Component)
├── [id].vue               # 編輯頁面 (Smart Component)
└── components/
    ├── {Module}Table.vue  # 表格組件 (Dumb Component)
    ├── {Module}Form.vue   # 表單組件 (Dumb Component)
    └── index.ts           # 組件導出
```

##### 核心特徵

- [ ] 列表頁 (index.vue)：
  - [ ] 使用 Table 組件（Dumb Component）
  - [ ] 處理所有業務邏輯（Smart Component）
  - [ ] 管理路由跳轉和刪除確認
- [ ] 表格組件 ({Module}Table.vue)：
  - [ ] 純展示組件（Dumb Component）
  - [ ] 使用 defineEmits 發送事件：create、edit、delete
  - [ ] 不包含任何業務邏輯或 Store 調用
- [ ] 編輯頁 ([id].vue)：
  - [ ] 使用 Form 組件（Dumb Component）
  - [ ] 處理新增/編輯邏輯（Smart Component）
- [ ] 表單組件 ({Module}Form.vue)：
  - [ ] 純展示組件（Dumb Component）
  - [ ] 使用 defineModel 實現雙向綁定
  - [ ] 提供 validateForm 方法
- [ ] Store：list、get、create、update、delete 方法

##### 程式碼模板

###### {Module}Table.vue 模板

```vue
<script setup lang="ts">
import { Delete, Plus } from "@element-plus/icons-vue";
import type { {Module}Item } from "~/types/{module}";

// Props
const props = defineProps<{
  data: {Module}Item[];
  loading?: boolean;
}>();

// Emits
const emit = defineEmits<{
  create: [];
  edit: [item: {Module}Item];
  delete: [item: {Module}Item];
}>();

// 表格行樣式
const tableRowClassName = () => {
  return "hover:cursor-pointer";
};

// 處理列点擊
const handleRowClick = (row: {Module}Item) => {
  emit("edit", row);
};

// 處理新增
const handleCreate = () => {
  emit("create");
};

// 處理刪除
const handleDelete = (row: {Module}Item) => {
  emit("delete", row);
};
</script>

<template>
  <div class="px-2">
    <el-table
      :data="data"
      v-loading="loading"
      stripe
      highlight-current-row
      :row-class-name="tableRowClassName"
      @row-click="handleRowClick"
      empty-text="無資料"
    >
      <!-- 動態欄位根據實際需求生成 -->
      <!-- 例如：<el-table-column prop="name" label="名稱" /> -->

      <!-- 操作欄 -->
      <el-table-column fixed="right" align="right" width="120">
        <template #header>
          <el-button type="primary" :icon="Plus" @click="handleCreate">
            新增
          </el-button>
        </template>
        <template #default="{ row }">
          <el-button
            type="danger"
            :icon="Delete"
            link
            @click.stop="handleDelete(row)"
          />
        </template>
      </el-table-column>
    </el-table>
  </div>
</template>
```

###### index.vue 模板（列表頁）

```vue
<script setup lang="ts">
import { use{Module}Store } from "~/stores/{module}";
import { {Module}Table } from "./components";
import type { {Module}Item } from "~/types/{module}";
import type ConfirmDialog from "~/components/ConfirmDialog.vue";

const {module}Store = use{Module}Store();
const router = useRouter();

const showDeleteConfirmDialog = ref(false);
const deleteConfirmDialog = ref<InstanceType<typeof ConfirmDialog>>();
const loading = ref(false);

// 載入列表資料
const loadData = async () => {
  loading.value = true;
  try {
    await {module}Store.list();
  } finally {
    loading.value = false;
  }
};

await loadData();

// 處理新增
const handleCreate = () => {
  router.push("/{module}/new");
};

// 處理編輯
const handleEdit = (item: {Module}Item) => {
  if (item.uId) {
    router.push(`/{module}/${item.uId}`);
  }
};

// 處理刪除
const handleDelete = async (uId: number) => {
  try {
    await {module}Store.delete(uId);
    ElNotification({
      title: "刪除成功",  // ← common.result.deleteSuccess
      type: "success",
    });
    await loadData(); // 重新載入
  } catch (error) {
    ElNotification({
      title: "刪除失敗",  // ← common.result.deleteFailed
      type: "error",
    });
  }
};

// 開啟刪除確認對話框
const openDeleteConfirmDialog = (item: {Module}Item) => {
  if (deleteConfirmDialog.value && item.uId) {
    deleteConfirmDialog.value?.setConfig({
      message: `您確定要刪除項目[${item.name || item.uId}]？`,  // ← 使用 common.action.confirmDeleteTemplate 格式
      onConfirm: () => handleDelete(item.uId!),
    });
    showDeleteConfirmDialog.value = true;
  }
};
</script>

<template>
  <div class="p-4">
    <div class="flex justify-between items-center mb-4">
      <h1 class="text-xl font-bold">{Module}管理</h1>
    </div>

    <{Module}Table :data="{module}Store.items" :loading="loading"
    @create="handleCreate" @edit="handleEdit" @delete="openDeleteConfirmDialog"
    />

    <ConfirmDialog
      ref="deleteConfirmDialog"
      v-model="showDeleteConfirmDialog"
    />
  </div>
</template>
```

###### {Module}Form.vue 模板

```vue
<script setup lang="ts">
import type { {Module}Item } from "~/types/{module}";

// Props
const props = withDefaults(
  defineProps<{
    loading?: boolean;
  }>(),
  {
    loading: false,
  }
);

// Model - 使用 defineModel 實現雙向綁定
const formData = defineModel<{Module}Item>("modelValue", {
  required: true,
});

// Form ref
const formRef = ref<FormInstance>();

// 驗證規則 - 優先使用 useIpValidators
import { useIpValidators } from "~/composables/useIpValidators";
const { ipv4Rule, domainRule } = useIpValidators();

const rules = reactive<FormRules>({
  // 根據實際需求定義
  // 例如：
  // name: [
  //   { required: true, message: "請輸入名稱", trigger: "blur" },
  // ],
  // ip: [
  //   { required: true, message: "必填", trigger: "blur" },
  //   ipv4Rule  // 使用標準 IP 驗證器
  // ]
});

// 暴露驗證方法給父組件
const validateForm = async () => {
  return await formRef.value?.validate();
};

// 重設表單
const resetForm = () => {
  formRef.value?.resetFields();
};

// 暴露方法
defineExpose({
  validateForm,
  resetForm,
});
</script>

<template>
  <el-form
    ref="formRef"
    :model="formData"
    :rules="rules"
    label-width="120px"
    v-loading="loading"
  >
    <!-- 動態表單欄位根據需求生成 -->
    <!-- 例如：
    <el-form-item label="名稱" prop="name">
      <el-input v-model="formData.name" />
    </el-form-item>
    
    <el-form-item label="啟用" prop="enabled">
      <el-switch v-model="formData.enabled" />
    </el-form-item>
    -->
  </el-form>
</template>
```

###### [id].vue 模板（編輯頁）

```vue
<script setup lang="ts">
import { use{Module}Store } from "~/stores/{module}";
import { {Module}Form } from "./components";
import type { {Module}Item } from "~/types/{module}";

const {module}Store = use{Module}Store();
const router = useRouter();
const route = useRoute();

const loading = ref(false);
const isNew = computed(() => route.params.id === "new");

// 表單資料
const formData = ref<{Module}Item>({
  // 初始化預設值
});

// 表單組件參考
const formComponent = ref<InstanceType<typeof {Module}Form>>();

// 載入資料（編輯模式）
const loadData = async () => {
  if (!isNew.value && route.params.id) {
    loading.value = true;
    try {
      const data = await {module}Store.get(Number(route.params.id));
      formData.value = data;
    } finally {
      loading.value = false;
    }
  }
};

await loadData();

// 儲存
const handleSave = async () => {
  const valid = await formComponent.value?.validateForm();
  if (!valid) return;

  loading.value = true;
  try {
    if (isNew.value) {
      await {module}Store.create(formData.value);
      ElNotification({
        title: "新增成功",  // ← common.result.addSuccess
        type: "success",
      });
    } else {
      await {module}Store.update(Number(route.params.id), formData.value);
      ElNotification({
        title: "更新成功",  // ← common.result.updateSuccess
        type: "success",
      });
    }
    router.push("/{module}");
  } catch (error) {
    ElNotification({
      title: isNew.value ? "新增失敗" : "更新失敗",  // ← common.result.addFailed/updateFailed
      type: "error",
    });
  } finally {
    loading.value = false;
  }
};

// 取消
const handleCancel = () => {
  router.push("/{module}");
};
</script>

<template>
  <div class="p-4">
    <el-card>
      <template #header>
        <h1 class="text-xl font-bold">{{ isNew ? "新增" : "編輯" }}{Module}</h1>
      </template>

      <{Module}Form ref="formComponent" v-model="formData" :loading="loading" />

      <div class="flex justify-end mt-4 space-x-4">
        <el-button type="info" @click="handleCancel">
          取消
          <!-- ← common.action.cancel -->
        </el-button>
        <el-button type="primary" @click="handleSave">
          儲存
          <!-- ← common.action.save -->
        </el-button>
      </div>
    </el-card>
  </div>
</template>
```

###### components/index.ts 模板

```typescript
export { default as {Module}Table } from "./{Module}Table.vue";
export { default as {Module}Form } from "./{Module}Form.vue";
```

#### Single-page Form 生成模板

##### 檔案結構

```
pages/{module}/
└── {feature}.vue      # 單一設定頁面
```

##### 核心特徵

- [ ] 單頁：`el-card` + `el-form` + 儲存/重設按鈕
- [ ] 無路由跳轉
- [ ] Store：getConfig、updateConfig 方法
- [ ] 通常用於系統設定、配置頁面

##### 程式碼模板

###### {feature}.vue 模板

```vue
<script setup lang="ts">
import { use{Module}Store } from "~/stores/{module}";
import type { {Module}Config } from "~/types/{module}";

const {module}Store = use{Module}Store();
const loading = ref(false);

// 表單資料
const formData = ref<{Module}Config>({
  // 初始化預設值
});

// 表單參考
const formRef = ref<FormInstance>();

// 表單驗證規則
const rules = reactive<FormRules>({
  // 根據需求定義驗證規則
});

// 載入設定
const loadConfig = async () => {
  loading.value = true;
  try {
    const config = await {module}Store.getConfig();
    formData.value = config;
  } finally {
    loading.value = false;
  }
};

// 儲存設定
const handleSave = async () => {
  const valid = await formRef.value?.validate();
  if (!valid) return;

  loading.value = true;
  try {
    await {module}Store.updateConfig(formData.value);
    ElNotification({
      title: "儲存成功",  // ← common.result.saveSuccess
      type: "success",
    });
  } catch (error) {
    ElNotification({
      title: "儲存失敗",  // ← common.result.saveFailed
      type: "error",
    });
  } finally {
    loading.value = false;
  }
};

// 重設表單
const handleReset = () => {
  loadConfig();
};

// 初始化
onMounted(() => {
  loadConfig();
});
</script>

<template>
  <div class="p-4">
    <el-card>
      <template #header>
        <h1 class="text-xl font-bold">{Module}設定</h1>
      </template>

      <el-form
        ref="formRef"
        :model="formData"
        :rules="rules"
        label-width="120px"
        v-loading="loading"
      >
        <!-- 動態表單欄位根據需求生成 -->
        <!-- 例如：
        <el-form-item label="啟用" prop="enabled">
          <el-switch v-model="formData.enabled" />
        </el-form-item>
        -->
      </el-form>

      <div class="flex justify-end mt-4 space-x-4">
        <el-button type="info" @click="handleReset"> 重設 </el-button>
        <el-button type="primary" @click="handleSave">
          儲存
          <!-- ← common.action.save -->
        </el-button>
      </div>
    </el-card>
  </div>
</template>
```

## 📋 執行檢查清單

### 執行前檢查（必須完成）

- [ ] [ ] **讀取工作檔案**：`.ai/workbench/issue-{number}.md` 存在且已讀取
- [ ] [ ] **路徑解析**：已從執行計劃或 API 中提取路徑
- [ ] [ ] **路徑映射**：已應用 PATH_MAPPINGS 或 API_TO_PATH 規則
- [ ] [ ] **UI 圖片**：已讀取並顯示給用戶
- [ ] [ ] **用戶確認**：已暫停並等待用戶確認路徑和模式

### 生成時檢查（必須確認）

- [ ] [ ] **UI 類型判斷**：基於 UI 圖片正確判斷模式
- [ ] [ ] **架構模式**：使用正確的頁面結構（Table-based vs Single-page）
- [ ] [ ] **檔案位置**：組件在 `pages/{module}/components/` 下
- [ ] [ ] **命名規範**：參考 `project-structure.md` 的命名規則
- [ ] [ ] **Smart/Dumb**：參考 `ui-standards.md` 的組件設計模式
- [ ] [ ] **程式碼模板**：使用本文檔中提供的模板
- [ ] [ ] **v-model 規範**：Dumb Component 使用 defineModel
- [ ] [ ] **通知方式**：使用 ElNotification 而非 ElMessage
- [ ] [ ] **❌ i18n 檢查**：確認沒有任何 $t()、t()、useI18n 的使用
- [ ] [ ] **文字檢查**：所有文字都是硬編碼中文
- [ ] [ ] **import 檢查**：沒有導入 vue-i18n
- [ ] [ ] **路徑確認**：生成到正確的目錄位置
- [ ] [ ] **組件拆分**：Table 和 Form 都是獨立的 Dumb Component
- [ ] [ ] **❌ placeholder 檢查**：確認沒有使用任何 placeholder attribute
- [ ] [ ] **通知檢查**：使用 ElNotification 而非 ElMessage

### 生成後驗證

執行完成後驗證：

```bash
# 驗證檔案結構
ls -la pages/{module}/
# 應該包含：index.vue, [id].vue, components/

ls -la pages/{module}/components/
# 應該包含：{Module}Table.vue, {Module}Form.vue, index.ts

# 驗證 Table 組件有使用 emit
grep "emit(" pages/{module}/components/{Module}Table.vue
# 應該找到：emit("create"), emit("edit", ...), emit("delete", ...)

# 驗證 Form 組件有使用 defineModel
grep "defineModel" pages/{module}/components/{Module}Form.vue
# 應該找到：defineModel 定義

# 驗證沒有使用 i18n
grep -E "\$t\(|useI18n|vue-i18n" pages/{module}/**/*.vue
# 不應該找到任何結果

# ⚠️ 驗證沒有違反 ui-standards.md 禁忌規範
echo "檢查是否違反 placeholder 禁用規範..."
if grep -q 'placeholder=' pages/{module}/**/*.vue; then
  echo "❌ 錯誤：發現使用 placeholder attribute"
  echo "違反 ui-standards.md:164 規範"
  echo "請移除所有 placeholder，改用 el-form-item 的 label"
  exit 1
fi

echo "檢查是否使用 ElMessage..."
if grep -q 'ElMessage' pages/{module}/**/*.vue; then
  echo "⚠️ 警告：發現使用 ElMessage"
  echo "根據 ui-standards.md，應該使用 ElNotification"
fi

echo "🎯 檢查 Common 文字使用規範..."

# 1. 檢查驗證訊息是否符合 common.validation 規範
FORBIDDEN_VALIDATION=(
  "請選擇" "請輸入" "請確認"
  "必須選擇" "必須輸入" "必須確認"
  "選擇.*必填" "輸入.*必填"
)
for pattern in "${FORBIDDEN_VALIDATION[@]}"; do
  if grep -qE "message.*${pattern}" pages/{module}/**/*.vue; then
    echo "❌ 錯誤：發現自創驗證訊息"
    echo "請使用 common.validation: 必填、數值必填、必須為數字、格式錯誤、驗證失敗"
    exit 1
  fi
done

# 2. 檢查是否使用非標準的操作文字
FORBIDDEN_ACTIONS=(
  "保存" "添加" "創建" "移除" "確認刪除項目"
  "保存成功" "創建成功" "操作成功"
  "保存失敗" "創建失敗" "操作失敗"
)
for pattern in "${FORBIDDEN_ACTIONS[@]}"; do
  if grep -qE "(title|>).*${pattern}" pages/{module}/**/*.vue; then
    echo "❌ 錯誤：發現自創操作文字: ${pattern}"
    echo "請使用 common 標準文字: 儲存、新增、刪除、儲存成功、新增成功等"
    exit 1
  fi
done

# 3. 檢查刪除確認訊息格式
if grep -qE "確定要刪除.*[^您確定要刪除.*\[.*\]？]" pages/{module}/**/*.vue; then
  echo "❌ 錯誤：刪除確認訊息格式不符合 common.action.confirmDeleteTemplate"
  echo "請使用格式: 您確定要刪除{type}[{name}]？"
  exit 1
fi

echo "✅ Common 文字使用規範檢查通過"
```

## 📂 參考規範層級架構

**依賴關係引用順序**：

1. **權威來源** (第一層)：
   - [ ] `.claude/rules/validation-mapping.md` - **Common 映射權威參考**

2. **實施規範** (第二層)：
   - [ ] `.claude/rules/ui-standards.md` - UI 開發標準（引用 validation-mapping.md）
   - [ ] `.claude/rules/project-structure.md` - 檔案命名、目錄結構
   - [ ] `.claude/rules/testing-guide.md` - 測試規範

3. **執行工具** (第三層)：
   - [ ] `i18n:extract` - 引用 validation-mapping.md 進行智能映射

> **重要**：此文件不再維護 Common 映射表，一律引用 `validation-mapping.md`

## 🧪 測試驗證

### 測試案例 1：Table-based Flow

```bash
/codegen:ui 704  # 防火牆管理（有列表+編輯圖）
```

**預期執行流程**：

1. 讀取 `.ai/workbench/issue-704.md`
2. 解析路徑：`system-settings/firewall` → `sys/firewall`
3. 讀取 UI 圖片並判斷為 Table-based Flow
4. **暫停確認**：路徑 `pages/sys/firewall/`，模式 Table-based Flow
5. 生成檔案：
   - [ ] `pages/sys/firewall/index.vue`
   - [ ] `pages/sys/firewall/[id].vue`
   - [ ] `pages/sys/firewall/components/FirewallTable.vue`
   - [ ] `pages/sys/firewall/components/FirewallForm.vue`
   - [ ] `pages/sys/firewall/components/index.ts`

### 測試案例 2：Single-page Form

```bash
# 假設 issue 只有單一設定頁面圖
/codegen:ui 705  # LDAP 設定
```

**預期執行流程**：

1. 讀取 `.ai/workbench/issue-705.md`
2. 解析路徑：`engineering/ldap` → `eng/ldap`
3. 讀取 UI 圖片並判斷為 Single-page Form
4. **暫停確認**：路徑 `pages/eng/`，模式 Single-page Form
5. 生成檔案：
   - [ ] `pages/eng/ldap.vue`

### 實際執行範例

#### 正確執行範例（包含所有步驟）

```typescript
// 1. 讀取工作檔案
const workfile = await Read(".ai/workbench/issue-704.md");

// 2. 解析路徑
const rawPath = "system-settings/firewall"; // 從執行計劃中提取
const mappedPath = "sys/firewall"; // 應用 PATH_MAPPINGS

// 3. 讀取 UI 圖片
const images = [
  await Read(".ai/workbench/issue-704/firewall-menu.png"),
  await Read(".ai/workbench/issue-704/firewall-edit.png"),
];

// 4. 判斷模式
const mode = "Table-based Flow"; // 有列表+編輯頁

// 5. 暫停等待確認
console.log(`
⏸️ **STOP HERE - 等待用戶確認**

**判斷結果**：Table-based Flow

**生成路徑**：pages/sys/firewall/

路徑解析過程：
- [ ] 原始參數：system-settings/firewall
- [ ] 映射規則：system-settings/* → sys/*
- [ ] 最終路徑：pages/sys/firewall/

**生成檔案**：
- [ ] pages/sys/firewall/index.vue
- [ ] pages/sys/firewall/[id].vue
- [ ] pages/sys/firewall/components/FirewallTable.vue
- [ ] pages/sys/firewall/components/FirewallForm.vue
- [ ] pages/sys/firewall/components/index.ts

請確認：
1. 路徑是否正確？ [輸入 y 或新路徑]
2. 模式是否正確？ [輸入 y 或 table/single]
`);

// 6. 獲得用戶確認後繼續
// ... 生成程式碼
```

### 用戶確認機制

#### 路徑確認

當路徑不確定時，必須詢問用戶：

```
根據分析，檔案將生成到：pages/{推斷的路徑}/

常見路徑選項：
1. pages/sys/{feature}/ - 系統設定類功能
2. pages/eng/{feature}/ - 工程設定類功能
3. pages/device/{feature}/ - 設備管理類功能
4. pages/{feature}/ - 獨立功能模組
5. 自訂路徑

請確認或輸入正確路徑：
```

#### 架構模式確認

當無法從 UI 圖片判斷時：

```
無法從 UI 圖片確定類型，請選擇：
1. table - Table-based Flow (列表+編輯)
2. single - Single-page Form (單一設定)
請輸入 [1/2/table/single]：
```

### 常見錯誤及解決方案

#### 錯誤 1：路徑生成錯誤

**問題**：生成到 `pages/firewall/` 而非 `pages/sys/firewall/`
**原因**：未應用路徑映射規則
**解決**：確保應用 PATH_MAPPINGS 或 API_TO_PATH

#### 錯誤 2：Table 組件未拆分

**問題**：Table 直接在 index.vue 中實現
**原因**：未使用程式碼模板
**解決**：使用本文檔提供的 {Module}Table.vue 模板

#### 錯誤 3：事件未正確綁定

**問題**：Table 的 edit/delete 事件不工作
**原因**：未使用 emit 機制
**解決**：確保 Table 組件使用 `emit("edit", item)` 等

## 版本歷史

- [ ] v1.0.0 (2025/10/20) 初始版本
