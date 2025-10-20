# Project Tdd Green

**指令**: `/project-tdd-green ['測試檔案或功能名稱']`
**版本**: v2.0.0
**功能**: 實作最簡單的程式碼讓測試通過
**可用工具**: Read, Edit, Bash - 建議限制使用這些工具

---
# TDD Green - 測試驅動開發：綠燈階段

## 🎯 用途

實作最簡單的程式碼讓測試通過，專注於功能實現而非優化，快速達到綠燈狀態。

## 📝 使用方法

`/project:tdd-green [測試檔案或功能名稱]`

## 🏗️ 核心原則

### KISS (Keep It Simple, Stupid)

- [ ] **最簡實作**：用最直接的方式通過測試
- [ ] **避免過早優化**：不要在綠燈階段做優化
- [ ] **清晰優於聰明**：寫出易懂的程式碼

### YAGNI (You Ain't Gonna Need It)

- [ ] **只寫需要的**：只實作測試要求的功能
- [ ] **不加額外功能**：不要預測未來需求
- [ ] **專注當下測試**：一次只通過一個測試

### SOLID 原則

- [ ] **暫不考慮**：綠燈階段先通過測試，重構階段再考慮 SOLID
- [ ] **保持簡單**：避免過度設計的類別結構
- [ ] **延遲決策**：架構決策留到重構階段

### DRY (Don't Repeat Yourself)

- [ ] **暫時容忍重複**：綠燈階段可以有重複程式碼
- [ ] **記錄重複位置**：為重構階段做準備
- [ ] **先完成再完善**：功能優先，優化其次

## 🔧 執行流程

### 1. 視覺化輸出

#### 執行開始時顯示

```
📊 Story 進度: ██████░░░░░░░░░░░░░░ 30% (1/3 AC 完成)
🎯 當前 AC 進度: ██████░░░░ 60% (3/5 Cycles 完成)

╔═══════════════════════════════════════════════════════════════════╗
║ 🟢 TDD GREEN PHASE - STORY-[編號]: [Story 標題]                   ║
║ AC-[編號]: [AC 描述]                                              ║
║ Cycle [當前/總數]: [Cycle 描述]                                   ║
╚═══════════════════════════════════════════════════════════════════╝

正在實作最簡單的程式碼通過測試...
```

### 2. 測試分析

- [ ] 載入失敗的測試：$ARGUMENTS
- [ ] 分析錯誤訊息
- [ ] 理解預期行為
- [ ] 確定實作範圍

### 3. 最小實作策略

#### 三種實作策略

1. **Fake it** - 先返回固定值（最簡單）
2. **Obvious implementation** - 明顯的實作（邏輯簡單時）
3. **Triangulation** - 透過多個測試推導（邏輯複雜時）

#### 選擇策略的原則 (KISS)

```python
# 策略 1: Fake it (最符合 KISS)
def add(a, b):
    return 5  # 如果測試只有 add(2,3)==5

# 策略 2: Obvious (邏輯簡單明顯)
def add(a, b):
    return a + b  # 簡單到不可能錯

# 策略 3: Triangulation (需要多個測試才能確定)
# 等第二個測試 add(1,1)==2 失敗後再改
```

### 4. 程式碼實作模式

#### 實作空函數/方法 (YAGNI)

```python
# Before (Red)
# AttributeError: 'TodoList' object has no attribute 'add_task'

# After (Green) - 只加需要的
class TodoList:
    def add_task(self, description):
        pass  # 最簡實作，只要方法存在即可
```

#### 處理測試需求 (KISS)

```python
# 測試要求: todo_list.count() 應該返回任務數量

# 最簡實作
class TodoList:
    def __init__(self):
        self.tasks = []  # 只因測試需要才加

    def add_task(self, description):
        self.tasks.append(description)  # 最簡單的實作

    def count(self):
        return len(self.tasks)  # 直接返回長度
```

### 5. 實作範例

#### 範例 1：認證功能 (KISS + YAGNI)

```python
# 測試: test_login_with_valid_credentials_returns_token
def login(email, password):
    # 最簡實作 - Fake it
    if email == "test@test.com" and password == "password":
        return {"success": True, "token": "fake-token"}
    return {"success": False}

# 原則體現：
# KISS ✓ - 硬編碼最簡單
# YAGNI ✓ - 沒有加密、沒有資料庫、沒有 JWT
```

#### 範例 2：資料儲存 (避免 SOLID 過度設計)

```python
# 測試: test_save_user_returns_id
class UserService:
    def __init__(self):
        self.users = []  # 記憶體儲存最簡單
        self.next_id = 1

    def save_user(self, name, email):
        user = {
            'id': self.next_id,
            'name': name,
            'email': email
        }
        self.users.append(user)
        self.next_id += 1
        return user['id']

# 原則體現：
# KISS ✓ - 不用資料庫，用 list
# YAGNI ✓ - 不做驗證、不做持久化
# 暫不考慮 SOLID - 不分離 Repository
```

### 6. 測試執行確認

```bash
# 執行特定測試
pytest test_story_001_add_task.py::test_add_command_exists -v

# 確認綠燈
✓ test_add_command_exists PASSED
```

#### 完成後顯示

```
╔═══════════════════════════════════════════════════════════════════╗
║ ✅ GREEN PHASE COMPLETE!                                          ║
║ 測試已通過：test_add_command_exists()                             ║
║ 下一步：執行 /project:tdd-refactor 進行重構                       ║
╚═══════════════════════════════════════════════════════════════════╝
```

### 7. 實作檢查清單

#### 綠燈階段檢查

- [ ] [ ] 測試是否通過？
- [ ] [ ] 是否最簡實作？(KISS)
- [ ] [ ] 有無多餘功能？(YAGNI)
- [ ] [ ] 能否更簡單？

#### 反面模式警示

```python
# ❌ 違反 KISS - 過度複雜
def calculate_discount(price, customer_type):
    # 不需要的設計模式
    strategy_map = {
        'regular': RegularDiscountStrategy(),
        'vip': VIPDiscountStrategy(),
        'new': NewCustomerStrategy()
    }
    return strategy_map[customer_type].calculate(price)

# ✅ 遵循 KISS - 簡單直接
def calculate_discount(price, customer_type):
    if customer_type == 'vip':
        return price * 0.8
    elif customer_type == 'new':
        return price * 0.9
    return price
```

## 💡 實作技巧

### 快速通過測試的優先順序

1. **Fake it** - 最快通過測試
2. **硬編碼** - 當測試案例少時
3. **簡單邏輯** - 當實作明顯時
4. **複製貼上** - 從測試複製預期值

### 何時停止

- [ ] 測試變綠 ✅
- [ ] 沒有編譯錯誤 ✅
- [ ] 功能符合測試要求 ✅
- [ ] 沒有額外功能 ✅

## 🚫 綠燈階段禁止事項

### 不要做的事

- [ ] ❌ 重構程式碼（等重構階段）
- [ ] ❌ 優化效能（YAGNI）
- [ ] ❌ 加註解（KISS）
- [ ] ❌ 處理邊界條件（除非測試要求）
- [ ] ❌ 設計架構（違反 KISS）

### 記住目標

```
目標：讓測試通過
不是：寫出完美程式碼
```

## 📚 相關指令

- [ ] `/project:tdd-red` - 撰寫失敗測試
- [ ] `/project:tdd-refactor` - 重構改善程式碼
- [ ] `/project:story-write` - 查看原始需求

## 🚨 注意

1. KISS 不要過度設計
2. 先做計劃,在我同意前不要進行任何操作,如果有不明白的地方要跟我討論
3. 務必深思熟慮 (ultrathink)

## ⚠️ 注意事項

- [ ] 抵抗過度設計的誘惑
- [ ] 相信測試會引導正確實作
- [ ] 保持耐心，重構階段再優化
- [ ] 一次只專注一個測試
- [ ] 記住：Simple is better than complex

## 版本歷史

- [ ] v2.0.0 (2025-08-17) 標準化格式，增加 allowed-tools，統一注意事項
- [ ] v1.0.0 (原始日期) 初始版本
