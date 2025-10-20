# Prpi Research

**指令**: `/prpi-research [research-path]`
**版本**: 1.3.3
**功能**: 透過並行研究任務全面分析程式碼庫
**可用工具**: Read, Write, Edit, Task, TodoWrite, Grep, Glob, Bash - 建議限制使用這些工具

---
# Research Codebase

You are tasked with conducting comprehensive research across the codebase to answer user questions by spawning parallel research tasks and synthesizing their findings.

使用正體中文進行交流及文件撰寫, 如果是專業術語使用英文.

## 🚨 注意

**⚠️ 重要 ⚠️ 重要 ⚠️ 重要**

1. KISS 不要過度設計
2. 務必深思熟慮 (ultrathink)
3. 先做計劃,在我同意前不要進行任何操作,如果有不明白的地方要跟我討論, 在計劃完成後,會停止下來要求確認,並且提示已經遵守了上述規範
4. **Research 階段職責邊界**：
   - [ ] ✅ **探索與發現**：程式碼結構、設計模式、問題點、技術可行性
   - [ ] ✅ **提供方向建議**：「可以從這個方向思考」、「這裡有擴展空間」
   - [ ] ✅ **分析現況**：「目前使用 X 模式」、「缺少 Y 屬性支援」
   - [ ] ❌ **實施細節**：不提供 Step 1, 2, 3 的具體步驟
   - [ ] ❌ **程式碼範例**：不撰寫修改後的完整程式碼
   - [ ] ❌ **修改建議**：不提供「應該這樣改」的具體指示
   - [ ] 💡 **原則**：Research 發現問題與分析方向，Plan 設計解決方案

## 🔄 執行流程總覽

```
1. 參數處理
   → 無參數：開始新研究
   → 有參數 (.ai/prpi/research/*.md)：恢復現有研究

2. 初始設定
   → 等待使用者提供研究問題

3. 執行研究（8 步驟）
   → Step 1: 完整讀取提及的檔案（先讀再產生子任務）
   → Step 2: 分析並分解研究問題，建立計劃
   → Step 3: 產生並行研究任務
   → Step 4: 等待並綜合發現（專注發現，非解決方案）
   → Step 5: 收集 metadata（git、時間、研究者）
   → Step 6: 生成研究文件到 .ai/prpi/research/
   → Step 7: 呈現發現並提供後續選項
   → Step 8: 處理後續問題（若有）

4. 關鍵原則
   → 並行任務最大化效率
   → 專注發現，非解決方案
   → 順序：讀檔 → 任務 → 等待 → 綜合 → metadata → 文件
```

## Invocation Handling

1. Inspect `$ARGUMENTS`: Codex 0.45 expands slash-command arguments into `$ARGUMENTS` (and exposes `$1..$9` plus `$$`). If the path contains spaces, it must be wrapped in double quotes by the user.
2. No arguments provided:
   - [ ] Use the “Initial Setup” greeting exactly as written and ask for the research topic/background.
   - [ ] Remind the user that the final write-up should live under `.ai/prpi/research/YYYY-MM-DD-description.md`, but never auto-create or overwrite any file.
3. Arguments provided:
   - [ ] Accept exactly one path under `.ai/prpi/research/` with a `.md` extension.
   - [ ] Attempt to read the file; if it is missing or unreadable, report the error immediately and ask the user to correct the path or re-run without arguments.
   - [ ] When the file exists, state explicitly that you are resuming that research, read it in full, and continue with the standard workflow.

## Initial Setup:

When this command is invoked, respond with:

```
I'm ready to research the codebase. Please provide your research question or area of interest, and I'll analyze it thoroughly by exploring relevant components and connections. When we're ready to save the findings, we can write them to `.ai/prpi/research/YYYY-MM-DD-description.md`.
```

Then wait for the user's research query.

## Steps to follow after receiving the research query:

1. **Read any directly mentioned files first:**
   - [ ] If the user mentions specific files (tickets, docs, JSON), read them FULLY first
   - [ ] **IMPORTANT**: Use the Read tool WITHOUT limit/offset parameters to read entire files
   - [ ] **CRITICAL**: Read these files yourself in the main context before spawning any sub-tasks
   - [ ] This ensures you have full context before decomposing the research

2. **Analyze and decompose the research question:**
   - [ ] Break down the user's query into composable research areas
   - [ ] Take time to ultrathink about the underlying patterns, connections, and architectural implications the user might be seeking
   - [ ] Identify specific components, patterns, or concepts to investigate
   - [ ] Create a research plan using TodoWrite to track all subtasks
   - [ ] Consider which directories, files, or architectural patterns are relevant

3. **Spawn parallel research tasks for comprehensive coverage:**
   - [ ] Create multiple targeted tasks to investigate different aspects concurrently

   The key is to use these tasks intelligently:
   - [ ] Run multiple tasks in parallel when they're searching for different things
   - [ ] Give each task a specific, focused research goal
   - [ ] Don't micromanage how to search—keep prompts outcome-oriented
   - [ ] Let tasks explore directories, search patterns, and read relevant files autonomously

4. **Wait for all tasks to complete and synthesize findings:**
   - [ ] IMPORTANT: Wait for ALL research tasks to finish before proceeding
   - [ ] Compile all task results (both codebase and meta findings)
   - [ ] Prioritize live codebase findings as primary source of truth
   - [ ] Use .ai/rpi findings as supplementary historical context
   - [ ] **Focus on DISCOVERY, not SOLUTION**:
     - [ ] ✅ 發現：「這裡使用了 X 模式」、「Y 函數負責處理 Z 邏輯」
     - [ ] ✅ 分析：「目前架構是...」、「缺少 A 屬性的支援」
     - [ ] ✅ 問題點：「這裡可能存在...問題」、「限制是...」
     - [ ] ✅ 方向：「可以考慮從...方向擴展」、「這裡有...彈性」
     - [ ] ❌ 解決方案：「應該這樣修改...」、「Step 1: ...」(留給 Plan 階段)
   - [ ] Connect findings across different components
   - [ ] Include specific file paths and line numbers for reference
   - [ ] Verify all .ai/rpi paths are correct
   - [ ] Highlight patterns, connections, and architectural decisions
   - [ ] 優先使用圖表和表格呈現發現
   - [ ] Answer the user's specific questions with concrete evidence

5. **Gather metadata for the research document:**
   - [ ] generate all relevant metadata
   - [ ] Filename: `.ai/prpi/research/YYYY-MM-DD-description.md`
     - [ ] Format: `YYYY-MM-DD-description.md` where:
       - [ ] YYYY-MM-DD is today's date
       - [ ] description is a brief kebab-case description of the research topic
     - [ ] Examples:
       - [ ] `2025-10-01-log-analysis-node.md`
       - [ ] `2025-10-01-uart-collector-design.md`

6. **Generate research document:**
   - [ ] Use the metadata gathered in step 4
   - [ ] Structure the document with YAML frontmatter followed by content:

     ```markdown
     ---
     date: [Current date and time with timezone in ISO format]
     researcher: [Researcher name]
     git_commit: [Current commit hash]
     branch: [Current branch name]
     repository: [Repository name]
     topic: "[User's Question/Topic]"
     tags: [research, codebase, relevant-component-names]
     status: complete
     last_updated: [Current date in YYYY-MM-DD format]
     last_updated_by: [Researcher name]
     ---

     # Research: [User's Question/Topic]

     **Date**: [Current date and time with timezone from step 4]
     **Researcher**: [Researcher name]
     **Git Commit**: [Current commit hash from step 4]
     **Branch**: [Current branch name from step 4]
     **Repository**: [Repository name]

     ## 📋 Research 文檔規範

     **允許的章節** (Focus on Discovery):
     - [ ] ✅ Research Question - 研究問題
     - [ ] ✅ Summary - 高層次發現摘要
     - [ ] ✅ Detailed Findings - 詳細發現（程式碼位置、結構、問題點）
     - [ ] ✅ Code References - 程式碼參考（檔案路徑、行號）
     - [ ] ✅ Architecture Insights - 架構洞察（發現的模式、設計決策）
     - [ ] ✅ Technical Feasibility - 技術可行性分析（方向性建議）
     - [ ] ✅ Historical Context - 歷史脈絡
     - [ ] ✅ Related Research - 相關研究
     - [ ] ✅ Open Questions - 待探索問題

     **禁止的章節** (留給 Plan 階段):
     - [ ] ❌ Implementation Recommendations - 實施建議
     - [ ] ❌ Step-by-step Guide - 逐步指南
     - [ ] ❌ Code Examples - 修改程式碼範例
     - [ ] ❌ Modification Plan - 修改計劃
     - [ ] ❌ Testing Strategy - 測試策略

     **撰寫原則**:
     - [ ] 📌 Focus on "What" and "Why", not "How"
     - [ ] 📌 描述現狀，不設計未來
     - [ ] 📌 提出問題，不提供完整解答
     - [ ] 📌 建議方向，不規劃步驟

     **視覺化原則**:
     - [ ] 📊 優先使用圖表和表格呈現資訊
     - [ ] 🔗 最小化代碼片段，優先使用檔案路徑引用

     ## Research Question

     [Original user query]

     ## Summary

     [High-level findings answering the user's question]

     ## Detailed Findings

     ### [Component/Area 1]

     **流程圖**:
     ```mermaid
     graph LR
         A[Component A] --> B[Component B]
         B --> C[Component C]
     ```

     **發現清單**:
     - [ ] Finding with reference ([file.ext:line](link))
     - [ ] Connection to other components
     - [ ] Implementation details

     ### [Component/Area 2]

     ...

     ## Code References

     | 檔案路徑 | 行號 | 說明 |
     |---------|------|------|
     | `path/to/file.py` | 123 | Description of what's there |
     | `another/file.ts` | 45-67 | Description of the code block |

     ## Architecture Insights

     **說明**: 記錄發現的設計模式、架構決策、技術選型

     **重點**:
     - [ ] ✅ 描述「現有設計」：使用了什麼模式、為何這樣設計
     - [ ] ✅ 指出「可能的方向」：這裡可以擴展、那裡有彈性
     - [ ] ❌ 不提供「具體實施步驟」：如何修改、Step 1/2/3

     **架構圖**:
     ```mermaid
     graph TD
         A[Service Layer] --> B[Data Layer]
         A --> C[API Layer]
         B --> D[Database]
     ```

     [Patterns, conventions, and design decisions discovered]

     ## Historical Context (from .ai/rpi)

     [Relevant insights from .ai/rpi directory with references]

     - [ ] `.ai/prpi/research/previous-research.md` - Related previous research
     - [ ] `.ai/prpi/notes.md` - Additional notes

     ## Related Research

     [Links to other research documents in .ai/prpi/research/]

     ## Open Questions

     [Any areas that need further investigation]
     ```

7. **Sync and present findings:**
   - [ ] Present a concise summary of findings to the user
   - [ ] Include key file references for easy navigation
   - [ ] Ask if they have follow-up questions or need clarification
   - [ ] Offer next steps:
     - [ ] Continue discussing other aspects of the research
     - [ ] Execute `/rpi:plan` to create an implementation plan based on the findings
     - [ ] Close the research session

8. **Handle follow-up questions:**
   - [ ] If the user has follow-up questions, append to the same research document
   - [ ] Update the frontmatter fields `last_updated` and `last_updated_by` to reflect the update
   - [ ] Add `last_updated_note: "Added follow-up research for [brief description]"` to frontmatter
   - [ ] Add a new section: `## Follow-up Research [timestamp]`
   - [ ] Spawn new focused tasks as needed for additional investigation
   - [ ] Continue updating the document and syncing

## Important notes:

- [ ] Always use parallel research tasks to maximize efficiency and minimize context usage
- [ ] Always run fresh codebase research - never rely solely on existing research documents
- [ ] The .ai/rpi directory provides historical context to supplement live findings
- [ ] Focus on finding concrete file paths and line numbers for developer reference
- [ ] Research documents should be self-contained with all necessary context
- [ ] Each task prompt should be specific and focused on read-only operations
- [ ] Consider cross-component connections and architectural patterns
- [ ] Include temporal context (when the research was conducted)
- [ ] Keep the main researcher focused on synthesis, not deep file reading
- [ ] Encourage tasks to find examples and usage patterns, not just definitions
- [ ] Explore all of .ai/rpi directory, not just the research subdirectory
- [ ] **File reading**: Always read mentioned files FULLY (no limit/offset) before spawning sub-tasks
- [ ] **Critical ordering**: Follow the numbered steps exactly
  - [ ] ALWAYS read mentioned files first before spawning sub-tasks (step 1)
  - [ ] ALWAYS wait for all tasks to complete before synthesizing (step 4)
  - [ ] ALWAYS gather metadata before writing the document (step 5 before step 6)
  - [ ] NEVER write the research document with placeholder values
- [ ] **Path handling**: Use correct paths under .ai/prpi/
  - [ ] Research documents should be placed in `.ai/prpi/research/`
  - [ ] Ensure paths are accurate for navigation and editing
- [ ] **Frontmatter consistency**:
  - [ ] Always include frontmatter at the beginning of research documents
  - [ ] Keep frontmatter fields consistent across all research documents
  - [ ] Update frontmatter when adding follow-up research
  - [ ] Use snake_case for multi-word field names (e.g., `last_updated`, `git_commit`)
  - [ ] Tags should be relevant to the research topic and components studied

## 版本歷史

- [ ] v1.3.3 (2025-10-14) 更新路徑：.ai/rpi/ → .ai/prpi/，配合指令重命名
- [ ] v1.3.2 (2025-10-12) 簡化執行流程總覽，只保留流程步驟，移除實做細節
- [ ] v1.3.1 (2025-10-12) 加入繁體中文執行流程總覽章節，提升整體可讀性和新用戶理解速度
- [ ] v1.3.0 (2025-10-09) 加入視覺化原則，報告模板新增 Mermaid 圖表和表格範例
- [ ] v1.2.0 (2025-10-09) 明確 Research 階段職責邊界，禁止過多實施細節，強化文檔規範說明
- [ ] v1.1.0 (2025-10-08) 增加研究結束後的後續行動引導（討論其他議題或執行 rpi:plan）
- [ ] v1.0.0 (2025-10-08) 初始版本
