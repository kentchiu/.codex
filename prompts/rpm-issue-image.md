---
version: 1.0.0
description: 下載 GitLab issue 中的所有圖片，智能檢查並避免重複下載
argument-hint: ISSUE=<issue-number>
---
# Rpm Issue Image

**可用工具**: Bash, Read, Write - 建議限制使用這些工具

---
# Issue Image Downloader


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

自動下載指定 GitLab issue 中的所有圖片，包含 issue 描述和所有討論中的圖片。智能檢查已存在的圖片完整性，避免重複下載。

**支援的圖片格式**：PNG, JPG, JPEG, GIF, BMP, SVG, WEBP
**排除的檔案**：PDF, DOC, DOCX, ZIP 等非圖片檔案

## 📝 使用方法

`/issue:image <issue_no>`

### 參數說明

- [ ] `<issue_no>`: GitLab issue 編號（例如：744）

### 輸出目錄

- [ ] 圖片儲存位置：`.ai/workbench/issue-<issue_no>/`
- [ ] 檔案命名格式：`{hash_id}_{original_filename}` (例如: `653a48209a95ab7d27349efccde21be6_image.png`)
- [ ] 自動建立目錄結構

## 🔧 執行步驟

### 1. 環境準備與設定

```bash
#!/bin/bash

# 載入環境變數
if [ -f ~/.dev.env ]; then
  source ~/.dev.env
fi

# 設定顏色輸出
RED='\033[0;31m'
GREEN='\033[0;32m'
YELLOW='\033[1;33m'
BLUE='\033[0;34m'
NC='\033[0m' # No Color

# 設定參數
ISSUE_NO="$1"
PROJECT_ID="91"  # rpm6/app/doc 專案 ID
GITLAB_URL="https://gitlab.bellwin.com.tw:11443"
GITLAB_TOKEN="${GITLAB_TOKEN:-glpat--5GHaMnXKyNykHnNTmEm}"

# 設定目錄
WORKBENCH_DIR=".ai/workbench/issue-${ISSUE_NO}"
mkdir -p "$WORKBENCH_DIR"

# 計數器
TOTAL_IMAGES=0
DOWNLOADED=0
SKIPPED=0
FAILED=0
```

### 2. 圖片完整性檢查函數

```bash
# 檢查圖片是否完整且可檢視
check_image_integrity() {
  local file_path="$1"
  
  # 檢查檔案是否存在
  if [ ! -f "$file_path" ]; then
    return 1
  fi
  
  # 檢查檔案大小（大於 0）
  if [ ! -s "$file_path" ]; then
    return 1
  fi
  
  # 使用 file 命令檢查是否為有效圖片
  if command -v file >/dev/null 2>&1; then
    local file_type=$(file -b --mime-type "$file_path")
    if [[ ! "$file_type" =~ ^image/ ]]; then
      return 1
    fi
  fi
  
  # 如果有 ImageMagick，使用 identify 進行深度檢查
  if command -v identify >/dev/null 2>&1; then
    if ! identify "$file_path" >/dev/null 2>&1; then
      return 1
    fi
  fi
  
  return 0
}
```

### 3. 從 GitLab API 獲取 Issue 內容

```bash
# 獲取 issue 詳細資訊
fetch_issue_content() {
  local issue_no="$1"
  
  echo -e "${BLUE}📋 獲取 Issue #${issue_no} 內容...${NC}"
  
  # 獲取 issue 主體
  local issue_response=$(curl -k -s -H "PRIVATE-TOKEN: $GITLAB_TOKEN" \
    "${GITLAB_URL}/api/v4/projects/${PROJECT_ID}/issues/${issue_no}")
  
  if [ -z "$issue_response" ] || [ "$issue_response" = "null" ]; then
    echo -e "${RED}❌ 無法獲取 Issue #${issue_no}${NC}"
    return 1
  fi
  
  # 保存 issue 內容供分析
  echo "$issue_response" > "$WORKBENCH_DIR/.issue_content.json"
  
  # 提取 issue 描述
  local description=$(echo "$issue_response" | jq -r '.description // ""')
  
  # 獲取所有討論
  echo -e "${BLUE}💬 獲取討論內容...${NC}"
  local discussions_response=$(curl -k -s -H "PRIVATE-TOKEN: $GITLAB_TOKEN" \
    "${GITLAB_URL}/api/v4/projects/${PROJECT_ID}/issues/${issue_no}/discussions?per_page=100")
  
  # 保存討論內容
  echo "$discussions_response" > "$WORKBENCH_DIR/.discussions.json"
  
  # 合併所有內容
  echo "$description" > "$WORKBENCH_DIR/.all_content.txt"
  echo "$discussions_response" | jq -r '.[].notes[].body // ""' >> "$WORKBENCH_DIR/.all_content.txt"
  
  return 0
}
```

### 4. 提取所有圖片 URL

```bash
# 從內容中提取所有圖片 URL
extract_image_urls() {
  local content_file="$1"
  local urls_file="$WORKBENCH_DIR/.image_urls.txt"
  
  echo -e "${BLUE}🔍 分析圖片連結...${NC}"
  
  # 清空 URL 列表
  > "$urls_file"
  
  # 提取 Markdown 圖片語法 ![](url)
  grep -oP '!\[.*?\]\(\K[^)]+(?=\))' "$content_file" | while read -r url; do
    # 過濾出圖片檔案
    if [[ "$url" =~ \.(png|jpg|jpeg|gif|bmp|svg|webp)(\?|$) ]]; then
      echo "$url" >> "$urls_file"
    fi
  done
  
  # 提取 HTML img 標籤
  grep -oP '<img[^>]+src=["'\'']\K[^"'\'']+' "$content_file" | while read -r url; do
    if [[ "$url" =~ \.(png|jpg|jpeg|gif|bmp|svg|webp)(\?|$) ]]; then
      echo "$url" >> "$urls_file"
    fi
  done
  
  # 提取 /uploads/ 路徑
  grep -oP '/uploads/[a-f0-9]+/[^"\s]+' "$content_file" | while read -r url; do
    echo "$url" >> "$urls_file"
  done
  
  # 去重並排序
  sort -u "$urls_file" -o "$urls_file"
  
  local count=$(wc -l < "$urls_file")
  echo -e "${GREEN}✅ 找到 ${count} 個圖片連結${NC}"
  
  TOTAL_IMAGES=$count
  return 0
}
```

### 5. 下載圖片主函數

```bash
# 下載單張圖片
download_image() {
  local url="$1"
  local retry_count=3
  local retry_delay=2
  
  # 處理 URL
  local full_url=""
  local filename=""
  
  # 處理不同類型的 URL
  if [[ "$url" =~ ^https?:// ]]; then
    full_url="$url"
    filename=$(basename "$url" | sed 's/?.*//')
  elif [[ "$url" =~ ^/uploads/ ]]; then
    # 轉換 /uploads/ 路徑為 API URL
    local path_parts=$(echo "$url" | sed 's|^/uploads/||')
    full_url="${GITLAB_URL}/api/v4/projects/${PROJECT_ID}/uploads/${path_parts}"
    
    # 提取 hash ID 和原始檔名
    local hash_id=$(echo "$url" | grep -oP '/uploads/\K[a-f0-9]+')
    local original_filename=$(basename "$url")
    filename="${hash_id}_${original_filename}"
  else
    # 相對路徑
    full_url="${GITLAB_URL}/${url}"
    filename=$(basename "$url")
  fi
  
  # 清理檔名（移除查詢參數）
  filename=$(echo "$filename" | sed 's/[?&].*//')
  local output_path="$WORKBENCH_DIR/$filename"
  
  # 檢查是否已存在且完整
  if [ -f "$output_path" ]; then
    if check_image_integrity "$output_path"; then
      echo -e "  ${GREEN}✓${NC} ${filename} (已存在且完整)"
      ((SKIPPED++))
      return 0
    else
      echo -e "  ${YELLOW}⚠${NC} ${filename} (檔案損壞，重新下載)"
      rm -f "$output_path"
    fi
  fi
  
  # 下載圖片（含重試機制）
  local attempt=1
  while [ $attempt -le $retry_count ]; do
    if curl -k -f -L \
      -H "PRIVATE-TOKEN: $GITLAB_TOKEN" \
      --connect-timeout 10 \
      --max-time 60 \
      -o "$output_path" \
      "$full_url" 2>/dev/null; then
      
      # 驗證下載的檔案
      if check_image_integrity "$output_path"; then
        local size=$(ls -lh "$output_path" | awk '{print $5}')
        echo -e "  ${GREEN}✓${NC} ${filename} (${size})"
        ((DOWNLOADED++))
        return 0
      else
        rm -f "$output_path"
      fi
    fi
    
    # 重試
    if [ $attempt -lt $retry_count ]; then
      sleep $retry_delay
      retry_delay=$((retry_delay * 2))
    fi
    attempt=$((attempt + 1))
  done
  
  echo -e "  ${RED}✗${NC} ${filename} (下載失敗)"
  ((FAILED++))
  return 1
}

# 批次下載所有圖片
download_all_images() {
  local urls_file="$WORKBENCH_DIR/.image_urls.txt"
  
  if [ ! -f "$urls_file" ] || [ ! -s "$urls_file" ]; then
    echo -e "${YELLOW}⚠️  沒有找到圖片${NC}"
    return 0
  fi
  
  echo -e "\n${BLUE}📥 開始下載圖片...${NC}"
  
  while IFS= read -r url; do
    download_image "$url"
  done < "$urls_file"
  
  return 0
}
```

### 6. 主程式

```bash
# 主函數
main() {
  if [ -z "$ISSUE_NO" ]; then
    echo -e "${RED}❌ 請提供 issue 編號${NC}"
    echo "使用方式: /issue:image <issue_no>"
    exit 1
  fi
  
  echo -e "${BLUE}╔════════════════════════════════════════╗${NC}"
  echo -e "${BLUE}║     GitLab Issue #${ISSUE_NO} 圖片下載器     ║${NC}"
  echo -e "${BLUE}╚════════════════════════════════════════╝${NC}"
  echo
  
  # 步驟 1: 獲取 issue 內容
  if ! fetch_issue_content "$ISSUE_NO"; then
    exit 1
  fi
  
  # 步驟 2: 提取圖片 URL
  extract_image_urls "$WORKBENCH_DIR/.all_content.txt"
  
  # 步驟 3: 下載所有圖片
  download_all_images
  
  # 顯示統計
  echo
  echo -e "${BLUE}╔════════════════════════════════════════╗${NC}"
  echo -e "${BLUE}║              📊 下載統計               ║${NC}"
  echo -e "${BLUE}╠════════════════════════════════════════╣${NC}"
  echo -e "${BLUE}║${NC} 總圖片數：    ${TOTAL_IMAGES} 個"
  echo -e "${BLUE}║${NC} ${GREEN}新下載：${NC}      ${DOWNLOADED} 個"
  echo -e "${BLUE}║${NC} ${YELLOW}已存在：${NC}      ${SKIPPED} 個"
  echo -e "${BLUE}║${NC} ${RED}失敗：${NC}        ${FAILED} 個"
  echo -e "${BLUE}╚════════════════════════════════════════╝${NC}"
  echo
  echo -e "${GREEN}✅ 圖片儲存於：${NC} $WORKBENCH_DIR/"
  
  # 列出所有圖片
  echo -e "\n${BLUE}📁 已下載的圖片：${NC}"
  ls -lh "$WORKBENCH_DIR"/*.{png,jpg,jpeg,gif,bmp,svg,webp} 2>/dev/null | awk '{print "   " $9 " (" $5 ")"}'
  
  # 如果有失敗的圖片，顯示提示
  if [ $FAILED -gt 0 ]; then
    echo -e "\n${YELLOW}💡 提示：${NC}"
    echo "  部分圖片下載失敗，可能原因："
    echo "  1. 圖片已被刪除或移動"
    echo "  2. 網路連線不穩定"
    echo "  3. 權限不足"
    echo "  可以再次執行指令重試"
  fi
}

# 執行主程式
main
```

## ⚠️ 注意事項

### 前置需求

1. **GitLab Token**：必須在 `~/.dev.env` 設定有效的 TOKEN
2. **網路連線**：能夠訪問 GitLab 伺服器
3. **工具依賴**：
   - [ ] `curl`: 下載工具
   - [ ] `jq`: JSON 處理
   - [ ] `file`: 檔案類型檢測（可選）
   - [ ] `identify`: ImageMagick 圖片驗證（可選）

### 圖片完整性檢查

系統會自動檢查圖片是否：
1. 檔案存在且大小 > 0
2. MIME 類型為圖片格式
3. 可被圖片處理工具識別

### 錯誤處理

- [ ] 自動重試失敗的下載（預設 3 次）
- [ ] 跳過已存在且完整的圖片
- [ ] 重新下載損壞的圖片
- [ ] 詳細的錯誤報告

## 📚 相關指令

- [ ] `/issue:start` - 開始處理 GitLab issue
- [ ] `/issue:view` - 檢視 issue 內容
- [ ] `/issue:sync` - 同步 issue 到本地

## 🔄 版本記錄

## 版本歷史
- [ ] v1.0.0 (2025/10/20) 初始版本
