---
version: 1.0.0
description: 分析程式碼並自動生成 D2 class diagram
argument-hint: TARGET="<file|directory|pattern>"
---
# D2 Class

**指令**: `/d2-class ['file | directory | pattern']`
**版本**: v1.0.0
**功能**: 分析程式碼並自動生成 class diagram
**可用工具**: Bash(d2 *), Write, Read, Bash(which d2), Bash(rm *), Grep, Glob, Task - 建議限制使用這些工具

---
# Code to Class Diagram

分析程式碼結構，自動生成 UML class diagram。

## 🚨 注意

**⚠️ 重要 ⚠️ 重要 ⚠️ 重要**

1. KISS 不要過度設計

## 核心邏輯

### 使用方式

```bash
/d2:class [file]          # 分析單一檔案的 classes
/d2:class [directory]     # 分析目錄下所有 classes
/d2:class [pattern]       # 使用 glob pattern (如 src/**/*.ts)
/d2:class                 # 分析當前目錄
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
   - [ ] 單一檔案：直接分析
   - [ ] 目錄：遞迴掃描相關檔案
   - [ ] Pattern：使用 glob 匹配
   - [ ] 無參數：當前目錄

3. **語言識別與解析**
   - [ ] TypeScript/JavaScript (.ts/.js/.tsx/.jsx)
   - [ ] Python (.py)
   - [ ] Java (.java)
   - [ ] C# (.cs)
   - [ ] Go (.go) - struct 為主
   - [ ] Rust (.rs) - struct/trait
   - [ ] Lua (.lua) - table/metatable

4. **類別結構分析**
   - [ ] **Classes/Structs**: 名稱、泛型參數
   - [ ] **Properties**: 名稱、型別、可見性
   - [ ] **Methods**: 名稱、參數、回傳型別、可見性
   - [ ] **Relationships**:
     - [ ] 繼承 (extends/implements)
     - [ ] 組合 (composition)
     - [ ] 聚合 (aggregation)
     - [ ] 依賴 (dependency)

5. **生成 D2 語法**
   - [ ] 轉換為 D2 class diagram 格式
   - [ ] 自動排版優化可讀性
   - [ ] 標註關係類型

6. **輸出圖檔**
   - [ ] 生成 .d2 檔案
   - [ ] 執行 `d2` 命令生成 SVG
   - [ ] 命名：`class-diagram-YYYYMMDD-HHMMSS.svg`

### 範例

#### 分析單一檔案

```bash
/d2:class models/user.ts
```

#### 分析整個專案

```bash
/d2:class src/
```

#### 使用 pattern

```bash
/d2:class "**/*.model.ts"
```

## 規則配置

### 語言特定解析

#### TypeScript/JavaScript

```typescript
class User {
  private id: number;
  public name: string;
  protected email: string;

  constructor() {}
  getName(): string {}
  static create(): User {}
}
```

#### Python

```python
class User(BaseModel):
    id: int
    name: str
    _email: str  # private

    def get_name(self) -> str:
        pass

    @classmethod
    def create(cls) -> 'User':
        pass
```

#### Java/C#

- [ ] 識別 class, interface, abstract class
- [ ] 解析 annotations/attributes
- [ ] 處理泛型

### 關係識別規則

1. **繼承**: extends, implements, :
2. **組合**: 類別作為屬性且生命週期綁定
3. **聚合**: 類別作為屬性但獨立生命週期
4. **依賴**: 方法參數或回傳型別

### D2 輸出格式

```d2
# Classes
User: {
  shape: class

  # Properties
  -id: number
  +name: string
  #email: string

  # Methods
  +getName(): string
  +create(): User {static}
}

BaseModel: {
  shape: class
  +validate(): boolean
}

Database: {
  shape: class
  +query(): Result
}

# Relationships
User -> BaseModel: extends
User -> Database: uses
```

### 可見性符號

- [ ] `+` public
- [ ] `-` private
- [ ] `#` protected
- [ ] `~` package/internal

## 錯誤處理

### D2 未安裝

```bash
# macOS/Linux
curl -fsSL https://d2lang.com/install.sh | sh

# 或使用套件管理器
brew install d2  # macOS
```

### 無法解析類別

- [ ] 檢查檔案語法
- [ ] 顯示支援的語言
- [ ] 提示簡化結構

### 圖表過於複雜

- [ ] 建議縮小範圍
- [ ] 提供過濾選項
- [ ] 分層展示

## 進階選項

### 過濾規則

- [ ] `--public-only`: 只顯示 public 成員
- [ ] `--no-private`: 隱藏 private 成員
- [ ] `--interfaces`: 只顯示介面
- [ ] `--depth N`: 限制關係深度

### 輸出控制

- [ ] 保留 .d2 原始檔供編輯
- [ ] 支援不同主題樣式
- [ ] 可調整圖表方向

## 版本歷史

- [ ] v1.0.0 (2025/10/20) 初始版本
