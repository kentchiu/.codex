# RPM Issue Image Script Wrapper

**指令**: `/rpm-issue-image-wrapper [issue-number]`
**版本**: wrapper-v1.0.0
**功能**: 執行 rpm-issue-image.sh 下載 GitLab issue 的圖片

> ⚠️ **RPM 專案專用**
> 此工具專為 rpm6/app/doc 專案設計。
>
> **依賴項目**:
> - GitLab API 訪問權限
> - 環境變數: `GITLAB_TOKEN`
> - curl, jq, ImageMagick (可選)
>
> **執行前檢查**:
> 1. 當前是否在 rpm 專案目錄中
> 2. GITLAB_TOKEN 環境變數是否已設定
> 3. 相關工具是否已安裝

---

## 使用方式

此 prompt 會幫助您執行 `rpm-issue-image.sh` script 來下載 GitLab issue 的圖片。

### 執行步驟

1. **確認環境變數**:
   ```bash
   echo $GITLAB_TOKEN
   ```
   如果未設定，請先設定：
   ```bash
   export GITLAB_TOKEN="your-token"
   ```

2. **執行 script**:
   ```bash
   bash /home/kent/cc2cx/codex/rpm-issue-image.sh [ISSUE_NUMBER]
   ```

3. **檢查結果**:
   圖片將下載到 `.ai/workbench/issue-[N]/` 目錄

### 範例

下載 issue #673 的圖片：
```bash
bash /home/kent/cc2cx/codex/rpm-issue-image.sh 673
```

### 說明

此 script 會：
- 從 GitLab API 獲取 issue 內容和討論
- 提取所有圖片 URL
- 批次下載圖片到工作目錄
- 驗證圖片完整性
- 提供下載統計

---

**請告訴我要處理哪個 issue number？**
