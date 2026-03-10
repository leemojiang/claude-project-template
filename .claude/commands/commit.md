---
name: commit
description: 提交当前未 commit 的修改。自动分析变更内容，生成规范的 commit message.
---

## 工作流程

### 1.查看未提交修改

git status --short

分析变更类型：
- M - 已修改
- ?? - 新文件（未跟踪）
- D - 已删除
- R - 重命名

git diff 


### 2.生成Commit Message
Commit Message的命名和内容遵循如下规则.


1. Commit type:
   - feat: new feature or capability
   - fix: bug fix
   - docs: documentation changes
   - refactor: code restructuring without behavior change
   - test: adding or modifying tests
   - chore: build, tooling, config, CI, non-code changes
   - style: formatting, whitespace, no logic changes
   - perf: performance improvements

2. Commit scope:
   - Infer from directory or file patterns (e.g., ui, api, docs, build, config)
   - If unclear, omit scope.

3. Commit title:
   - Short (≤ 72 chars)
   - Imperative mood
   - Summarize the main change

4. Commit body:
   - Summaries of key modifications
   - Why the change was needed
   - Optional: risks, notes, or follow-up tasks

### 3.执行提交

git add <file1> <file2> ...
git commit -m "commit message"

注意：
- 避免使用 git add . 或 git add -A
- 明确指定要提交的文件
- 排除临时文件（.bak-*、.html.bak-*）