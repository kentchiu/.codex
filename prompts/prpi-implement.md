# Prpi Implement

**指令**: `/prpi-implement <plan-path>`
**版本**: 1.4.2
**功能**: 實施已批准的技術計劃，按階段執行並驗證成功標準
**可用工具**: Read, Write, Edit, Bash, TodoWrite, Task - 建議限制使用這些工具

---
# Implement Plan

You are tasked with implementing an approved technical plan from `.ai/prpi/plans/`. These plans contain phases with specific changes and success criteria.

使用正體中文進行交流及文件撰寫, 如果是專業術語使用英文.

## 🚨 注意

**⚠️ 重要 ⚠️ 重要 ⚠️ 重要**

1. KISS 不要過度設計
2. 務必深思熟慮 (ultrathink)
3. 先做計劃,在我同意前不要進行任何操作,如果有不明白的地方要跟我討論, 在計劃完成後,會停止下來要求確認,並且提示已經遵守了上述規範
4. 必須提供有效的計劃檔案路徑 (`.ai/prpi/plans/*.md`)
5. 實施前請完整閱讀計劃檔案和相關文件
6. 按階段執行，完成一個階段後再進行下一個
7. 自動驗證通過後，需暫停等待人工確認手動測試
8. 遇到與計劃不符的情況時，立即停止並尋求指導
9. **🔴 必須執行**：完成所有階段後，**必須自動生成實施報告**到 `.ai/prpi/implementations/`，這不是可選步驟

## 🔄 執行流程總覽

```
1. 參數處理
   → 無參數：要求提供計劃檔案路徑
   → 有參數 (.ai/prpi/plans/*.md)：驗證並讀取計劃

2. Git 狀態檢查（依序執行）
   → 檢查工作區狀態（uncommitted changes？）
   → 檢查當前分支（main/master？建議建立新分支）
   → 取得使用者確認後才繼續

3. 開始實施
   → 完整讀取計劃檔案與相關文件
   → 檢查現有 checkmarks（恢復工作點）
   → 建立 todo list 追蹤進度
   → 開始實施

4. 按階段執行
   → 實施每個 item（遵循計劃意圖，適應實際狀況）
   → 完成 item 後立即更新 checkbox
   → 執行相關測試並修正問題
   → 完成階段後執行完整驗證

5. 階段驗證
   → 執行自動驗證（make check test）
   → 暫停等待人工確認手動測試
   → 取得確認後進入下一階段

6. 遇到問題處理
   → 停止並清楚呈現問題
   → 尋求指導後再繼續

7. 完成 (🔴 必須執行所有步驟)
   → 確認所有階段完成
   → 收集 metadata (git commit, branch, date, implementer)
   → 使用 Write 工具生成實施報告到 .ai/prpi/implementations/YYYY-MM-DD-*.md
   → 呈現報告完整路徑給使用者
   → 總結實施內容與偏差
   → 提供後續選項（討論/新工作/結束）
```

## Invocation Handling

1. Inspect `$ARGUMENTS` (Codex 0.45 passes a single path, supports `$1..$9` and `$$`; paths with spaces must be double-quoted).
2. No arguments: ask the user for the plan file path, or direct them to create one via `/plan` first.
3. Arguments present:
   - [ ] Accept only `.ai/prpi/plans/*.md`.
   - [ ] Attempt to read the file; if it is missing or invalid, report the issue and tell the user to create/fix the plan before continuing.
   - [ ] When the file exists, confirm that you will continue implementing that plan and follow the "Git Status Check" steps.

## Git Status Check

Before starting implementation, verify git repository state in the following order:

1. **Check working directory status first**:
   - [ ] Run `git status --porcelain`
   - [ ] If there are uncommitted changes:
     - [ ] Inform: "You have uncommitted changes in your working directory"
     - [ ] Ask: "Would you like to commit these changes before proceeding?"
     - [ ] If yes: Help create a commit or guide manual commit
     - [ ] If no: Warn that implementation may create conflicts

2. **Then check current branch**:
   - [ ] Run `git branch --show-current`
   - [ ] If on `main` or `master`:
     - [ ] Ask: "You are on the {branch} branch. Would you like to create a new branch for this implementation?"
     - [ ] If yes: Extract a simple name from the plan filename (remove version, date, prefixes) and create the branch
     - [ ] If no: Confirm user wants to proceed on {branch} (not recommended)

3. **Proceed to Getting Started** only after git status is acceptable or user explicitly approves

## Getting Started

When given a plan path:

- [ ] Confirm the file exists under `.ai/prpi/plans/`; if the read fails, ask the user to fix the path or complete `/plan` first
- [ ] Read the plan completely and check for any existing checkmarks (- [x])
- [ ] Read the original ticket and all files mentioned in the plan
- [ ] **Read files fully** - never use limit/offset parameters, you need complete context
- [ ] Think deeply about how the pieces fit together
- [ ] Create a todo list to track your progress
- [ ] Start implementing if you understand what needs to be done

If no plan path provided, ask for one.

## Implementation Philosophy

Plans are carefully designed, but reality can be messy. Your job is to:

- [ ] Follow the plan's intent while adapting to what you find
- [ ] Implement each phase fully before moving to the next
- [ ] Verify your work makes sense in the broader codebase context
- [ ] Update checkboxes in the plan as you complete sections

When things don't match the plan exactly, think about why and communicate clearly. The plan is your guide, but your judgment matters too.

If you encounter a mismatch:

- [ ] STOP and think deeply about why the plan can't be followed
- [ ] Present the issue clearly:

  ```
  Issue in Phase [N]:
  Expected: [what the plan says]
  Found: [actual situation]
  Why this matters: [explanation]

  How should I proceed?
  ```

## Verification Approach

**重要：逐項更新進度**

- [ ] 每完成一個 item，立即使用 Edit 工具更新該 item 的 checkbox
- [ ] 不要等到多個 items 完成才批次更新
- [ ] 這讓用戶可以即時看到進度

After implementing each item:

- [ ] Run relevant tests if applicable
- [ ] Fix any issues before proceeding to next item
- [ ] Immediately check off the completed item in the plan file using Edit
- [ ] Update your todos to reflect current progress

After completing a phase:

- [ ] Run the full success criteria checks for the phase (usually `make check test` covers everything)
- [ ] Verify all checkboxes in the phase are checked
- [ ] **Pause for human verification**: After completing all automated verification for a phase, pause and inform the human that the phase is ready for manual testing. Use this format:

  ```
  Phase [N] Complete - Ready for Manual Verification

  Automated verification passed:
  - [ ] [List automated checks that passed]

  Please perform the manual verification steps listed in the plan:
  - [ ] [List manual verification items from the plan]

  Let me know when manual testing is complete so I can proceed to Phase [N+1].
  ```

If instructed to execute multiple phases consecutively, skip the pause until the last phase. Otherwise, assume you are just doing one phase.

do not check off items in the manual testing steps until confirmed by the user.

## If You Get Stuck

When something isn't working as expected:

- [ ] First, make sure you've read and understood all the relevant code
- [ ] Consider if the codebase has evolved since the plan was written
- [ ] Present the mismatch clearly and ask for guidance

Use sub-tasks sparingly - mainly for targeted debugging or exploring unfamiliar territory.

## Resuming Work

If the plan has existing checkmarks:

- [ ] Trust that completed work is done
- [ ] Pick up from the first unchecked item
- [ ] Verify previous work only if something seems off

Remember: You're implementing a solution, not just checking boxes. Keep the end goal in mind and maintain forward momentum.

## ⚠️ CRITICAL: After All Phases Complete

**IMPORTANT**: As soon as all phases are marked complete and verified, you **MUST immediately proceed** to the Completion section below.

**Do NOT stop at just providing a summary** - you **MUST generate the implementation document** to `.ai/prpi/implementations/`. This is a **required step**, not optional.

## Completion

**REQUIRED STEPS** - You MUST complete ALL of these steps when all phases are done:

1. **Confirm completion** with the user:
   - [ ] Summarize what was implemented
   - [ ] List all phases completed
   - [ ] Note any deviations from the original plan

2. **REQUIRED: Gather metadata for the implementation document**:
   - [ ] Current date and time with timezone (ISO format)
   - [ ] Git commit hash: `git rev-parse HEAD`
   - [ ] Git branch: `git branch --show-current`
   - [ ] Repository name
   - [ ] Implementer name (from git config or context)
   - [ ] Original plan path

3. **REQUIRED: Generate implementation document using Write tool**:
   - [ ] Filename: `.ai/prpi/implementations/YYYY-MM-DD-description.md`
     - [ ] Format: `YYYY-MM-DD-description.md` where:
       - [ ] YYYY-MM-DD is today's date
       - [ ] description is a brief kebab-case description matching the plan
     - [ ] Examples:
       - [ ] `2025-10-13-parent-child-tracking.md`
       - [ ] `2025-10-13-improve-error-handling.md`
   - [ ] Use the following template structure:

     ```markdown
     ---
     date: [Current date and time with timezone in ISO format]
     implementer: [Implementer name]
     git_commit: [Current commit hash]
     branch: [Current branch name]
     repository: [Repository name]
     plan: [Original plan path]
     tags: [implementation, feature-name, relevant-components]
     status: complete
     last_updated: [Current date in YYYY-MM-DD format]
     last_updated_by: [Implementer name]
     ---

     # Implementation: [Feature/Task Name]

     **Date**: [Current date and time with timezone]
     **Implementer**: [Implementer name]
     **Git Commit**: [Current commit hash]
     **Branch**: [Current branch name]
     **Repository**: [Repository name]
     **Original Plan**: [Plan path]

     ## Summary

     [High-level summary of what was implemented]

     ## Phases Completed

     ### Phase 1: [Phase Name]

     **Changes Made**:
     - [ ] [List of actual changes made]
     - [ ] [File paths and modifications]

     **Verification Results**:
     - [ ] ✅ Automated: [List of passed automated checks]
     - [ ] ✅ Manual: [Confirmation of manual testing]

     ### Phase 2: [Phase Name]

     [Similar structure...]

     ## Deviations from Plan

     [Document any deviations from the original plan and why they were necessary]

     ## Challenges & Solutions

     [Any challenges encountered and how they were resolved]

     ## Testing Summary

     **Automated Tests**:
     - [ ] [Test results summary]

     **Manual Tests**:
     - [ ] [Manual testing confirmation]

     ## Code References

     | 檔案路徑 | 說明 |
     |---------|------|
     | `path/to/file.py` | Description of changes |
     | `another/file.ts` | Description of changes |

     ## Next Steps

     [Any follow-up work or improvements identified during implementation]

     ## Reflection

     [Any insights or lessons learned during implementation]
     ```

4. **REQUIRED: Present completion and offer next steps**:
   - [ ] **Show the full implementation document path** (e.g., `.ai/prpi/implementations/2025-10-14-feature-name.md`)
   - [ ] Summarize what was accomplished
   - [ ] Ask if they want to:
     - [ ] Discuss any follow-up work or improvements
     - [ ] Create a new research or plan for related work
     - [ ] Close the implementation session

**VERIFICATION**: After completing step 3, you MUST have created a file in `.ai/prpi/implementations/`. If you haven't, you did NOT complete the implementation properly.

## 版本歷史

- [ ] v1.4.2 (2025-10-14) 🔴 修正報表生成問題：加強 Completion 章節的強制性指示，確保完成後必定生成實施報告
- [ ] v1.4.1 (2025-10-14) 更新路徑：.ai/rpi/ → .ai/prpi/，配合指令重命名
- [ ] v1.4.0 (2025-10-13) 新增完整的實施報告生成步驟，包含 metadata 收集和文件模板
- [ ] v1.3.2 (2025-10-12) 加入簡化的執行流程總覽章節，提升整體可讀性
- [ ] v1.3.1 (2025-10-11) 調整 Git 檢查順序：先檢查工作區，再檢查分支，最後才開始實施
- [ ] v1.3.0 (2025-10-11) 新增 Git 狀態檢查，確保在正確分支且工作區乾淨後才開始實施
- [ ] v1.2.0 (2025-10-09) 修正 checkbox 更新時機，明確要求每完成一個 item 立即更新
- [ ] v1.1.0 (2025-10-08) 增加實施完成後的後續行動引導（討論、新工作、結束）
- [ ] v1.0.0 (2025-10-08) 初始版本
