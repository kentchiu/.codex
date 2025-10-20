# D2 Sequence

**指令**: `/d2-sequence ['file#method | file | directory']`
**版本**: v1.0.0
**功能**: 分析程式碼並自動生成 sequence diagram
**可用工具**: Bash(d2 *), Write, Read, Bash(which d2), Bash(rm *), Grep, Glob, Task - 建議限制使用這些工具

---
# Code to Sequence Diagram

分析程式碼結構，自動生成 UML sequence diagram。

## 🚨 注意

**⚠️ 重要 ⚠️ 重要 ⚠️ 重要**

1. KISS 不要過度設計

## 核心邏輯

### 使用方式

```bash
/d2:sequence [file#method]  # 分析指定檔案的特定 method
/d2:sequence [file]         # 列出檔案所有 methods 供選擇
/d2:sequence [directory]    # 掃描目錄找入口點
/d2:sequence               # 使用當前目錄
```

**❌ 避免使用**：

- [ ] Text blocks: `|md`, `|latex`, `|`
- [ ] Legend 語法

**KISS 原則**：保持圖表簡潔易讀，專注核心邏輯關係。

### 執行流程

1. **檢查 D2 CLI 是否安裝**
   - [ ] 使用 `which d2` 確認
   - [ ] 若未安裝，提供安裝指引

2. **解析輸入參數**
   - [ ] `file#method`: 直接分析指定 method
   - [ ] `file`: 解析檔案，列出所有 methods
   - [ ] `directory` 或無參數: 掃描目錄結構

3. **語言識別與解析**
   - [ ] 根據副檔名識別語言
   - [ ] 支援: TypeScript/JavaScript (.ts/.js/.tsx/.jsx)
   - [ ] 支援: Python (.py)
   - [ ] 支援: Go (.go)
   - [ ] 支援: Rust (.rs)
   - [ ] 支援: Java (.java)
   - [ ] 支援: C# (.cs)
   - [ ] 支援: Lua (.lua)

4. **程式碼分析**
   - [ ] 找出 functions/methods 定義
   - [ ] 追蹤呼叫關係（2-3 層深度）
   - [ ] 識別外部函式庫呼叫（標註但不深入）
   - [ ] 偵測條件分支和迴圈（簡化標註）

5. **生成 D2 語法**
   - [ ] 將呼叫鏈轉為 sequence diagram
   - [ ] 自動識別 actors（classes/modules）
   - [ ] 標註參數和返回值（簡化）

6. **輸出圖檔**
   - [ ] 生成 .d2 檔案
   - [ ] 執行 `d2` 命令生成 SVG
   - [ ] 時間戳命名：`sequence-[method].svg`

### 範例

#### 分析特定 method

```bash
/d2:sequence server.ts#handleRequest
```

#### 選擇檔案中的 methods

```bash
/d2:sequence auth.py
# 輸出:
# 1. login()
# 2. logout()
# 3. validateToken()
# 選擇: 1,3
```

#### 掃描目錄

```bash
/d2:sequence src/
# 自動找出 main(), index.*, app.* 等入口點
```

## 規則配置

### 語言解析策略

#### TypeScript/JavaScript

- [ ] 使用正則或簡單 AST 解析
- [ ] 識別: function, arrow functions, class methods
- [ ] 追蹤: 函數呼叫、物件方法呼叫

#### Python

- [ ] 使用縮排和關鍵字識別
- [ ] 識別: def, async def, class methods
- [ ] 追蹤: 函數呼叫、模組引用

#### 其他語言

- [ ] Go: func 關鍵字
- [ ] Rust: fn 關鍵字
- [ ] Java/C#: method signatures
- [ ] Lua: function 關鍵字

### 分析深度控制

```yaml
預設深度: 2-3 層
外部函式庫: 停止追蹤，標註為 [External]
遞迴呼叫: 標註但不展開
```

### 入口點識別規則

優先順序：

1. main 函數
2. index.\* 檔案的預設匯出
3. app._ 或 server._ 檔案
4. API endpoints (get/post/put/delete)
5. event handlers (on*, handle*)

### D2 輸出格式

```d2
shape: sequence_diagram

# Actors (自動識別)
Client
Server
Database
[External Library]

# Interactions
Client -> Server: handleRequest(data)
Server -> Database: query(sql)
Database -> Server: results
Server -> [External Library]: process()
[External Library] -> Server: processed
Server -> Client: response
```

## 錯誤處理

### D2 未安裝

```bash
# macOS/Linux
curl -fsSL https://d2lang.com/install.sh | sh

# 或使用套件管理器
brew install d2  # macOS
```

### 無法解析程式碼

- [ ] 顯示支援的語言列表
- [ ] 提示檢查檔案格式
- [ ] 建議簡化程式碼結構

### 找不到 methods

- [ ] 列出找到的檔案
- [ ] 提示可能的入口點
- [ ] 建議指定特定檔案

## 版本歷史

- [ ] v1.0.0 (2025-08-31) 初始版本 - 手動 D2 語法
