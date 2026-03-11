---
name: commit-skill
description: 提交当前 Git 仓库里所有未提交的修改：先用 git status/diff 分析变更内容并生成规范的 Conventional Commits commit message（type/scope/title/body），再明确列出要提交的文件并逐个 git add / git rm，最后执行 git commit。用于用户要求“帮我提交修改/生成 commit message/按规范提交/不要 git add .”等场景。
---

# Commit

## 工作流程

目标：在不误提交临时文件/无关文件的前提下，生成清晰、可追溯的 commit message，并完成一次提交。

### 1) 查看未提交修改

1. 查看工作区状态（用于确定要提交的文件清单与变更类型）：

```bash
git status --short
```

变更类型速记：
- `M`：已修改
- `A` / `??`：新增文件（`??` 为未跟踪）
- `D`：已删除
- `R`：重命名

2. 查看改动内容（用于生成 message；必要时再细看单文件 diff）：

```bash
git diff
```

对未跟踪文件（`??`）需要额外查看内容时：

```bash
git diff --no-index -- /dev/null path/to/new_file
```

过滤规则（避免误提交）：
- 不要提交临时/备份文件：`*.tmp`、`.bak-*`、`.html.bak-*`（以及类似明显的生成物）。
- 如果仓库里存在自动生成目录（如 `dist/`、`build/`、`.pytest_cache/`），除非用户明确要求，否则不要纳入本次提交。

如果 `git status --short` 为空：直接说明“无可提交修改”，不要创建空提交。

### 2) 生成规范 Commit Message（Conventional Commits）

Commit message 格式：

```
<type>(<scope>): <title>

<body>
```

#### 2.1 type（必填）

从变更内容推断（择其最主要的一类）：
- `feat`: 新功能/新能力
- `fix`: 修复 bug
- `docs`: 文档变更
- `refactor`: 重构（不改变行为）
- `test`: 新增/修改测试
- `chore`: 构建/工具链/配置/CI/非业务代码
- `style`: 格式化/空白/无逻辑变化
- `perf`: 性能优化

不确定时，优先使用更保守的 `chore` / `refactor` / `docs`，避免把“非功能变更”误标成 `feat`。

#### 2.2 scope（可选）

从目录/领域推断：
- `src/` → `core`（或更具体子模块名）
- `tests/` → `test`
- `scripts/` → `scripts`
- `config/` / `pyproject.toml` / CI 文件 → `config` / `build`
- 变更横跨多个领域且无法归一：省略 scope

#### 2.3 title（必填）

规则：
- ≤ 72 字符（英文场景）；中文尽量短
- 祈使句/动词开头（如 “Add… / Fix… / Update…” 或 “新增… / 修复… / 调整…”）
- 只写“最核心的一件事”

#### 2.4 body（推荐）

用 2–6 行概括：做了什么 + 为什么（必要时补充风险/后续）。示例：

```
- Summarize key changes
- Explain motivation/why (if non-obvious)
```

### 3) 明确暂存文件并执行提交

原则：只暂存本次提交需要的文件；避免使用 `git add .` 或 `git add -A`。

1. 根据 `git status --short` 列出要提交的文件清单，并逐个暂存：

```bash
git add path/to/file1
git add path/to/file2
```

对删除文件使用：

```bash
git rm path/to/deleted_file
```

2. 提交前复核暂存区：

```bash
git diff --cached
```

3. 执行提交（标题 + 正文；没有正文也可只用第一条 `-m`）：

```bash
git commit -m "type(scope): title" -m "body line 1" -m "body line 2"
```

如果本次包含明显不同主题的改动：不要硬塞进一个提交；拆分为多个小提交，每个提交有独立 message 与文件清单。
