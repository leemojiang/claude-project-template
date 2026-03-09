markdown
# AI 项目管理工作流程（Workflow Contract）

本文件定义 AI 在本项目中的固定工作流程、目录结构、文档格式、审批机制与变更记录规范。AI 必须严格遵守本协议执行所有任务。

---

## 1. 目录结构规范

.ai/
├── conventions/     # 全局规定、设计规范、命名规范、流程协议
├── design/          # 项目设计文档（整体 + 分模块）
│     ├── overview.md
│     └── components/*.md
├── plan/            # AI 执行计划（每任务一个文件）
├── modify/ (或 rfc/) # 变更记录（diff、原因、影响范围）
└── review/          # Review 结果与测试结果


---

## 2. AI 的工作流程（必须遵守）

AI 在执行任何任务时必须遵循以下顺序：

### Step 1 — 文档更新（Document Update）
- AI 必须先更新 `.ai/design` 或 `.ai/plan` 中的相关文档。
- 文档更新后必须停止，等待用户批准。
- 未经批准不得进入下一阶段。

### Step 2 — 实施（Implementation）
- 用户批准后，AI 才能执行代码修改、创建文件、重构等操作。
- 实施必须严格遵守 `.ai/conventions` 中的规范。

### Step 3 — 记录变更（Change Log）
- 实施完成后，AI 必须在 `.ai/modify`（或 `.ai/rfc`）中生成变更记录。
- 变更记录必须包含完整 diff、修改原因、影响范围、风险点。

### Step 4 — Review & Test
- AI 必须在 `.ai/review` 中生成 review 文档。
- 内容包括自我 review、测试结果、规范符合性检查。
- 若发现问题，AI 必须提出修复计划并回到 Step 1。

---

## 3. 各目录职责与文档格式

### 3.1 `.ai/conventions/` — 全局规定
用于存放：

- 项目编码规范
- 架构原则
- 命名规范
- 目录结构约定
- 本 workflow 协议
- 文档格式规范

AI 必须在每次规划前读取此目录内容。

---

### 3.2 `.ai/design/` — 项目设计文档

#### overview.md
- 项目目标
- 架构概览
- 模块划分
- 数据流 / 控制流
- 关键技术点

#### components/*.md
每个模块一个文件，比如：

.ai/design/components/auth.md
.ai/design/components/database.md
.ai/design/components/api.md

内容包括:

- 模块职责
- 输入 / 输出
- 依赖关系
- 数据结构
- 扩展点

AI 的义务：
    在每次新增功能前必须更新相关设计文件
    若设计变更，必须先更新 .ai/design 再等待你批准

---

### 3.3 `.ai/plan/` — 执行计划
每次任务一个文件，例如：

.ai/plan/task-20260309-1.md

内容包括：
- 任务目标
- 需要修改的文件列表
- 预期输出
- 风险点
- 执行步骤（Step-by-step）

AI 的义务：
    每次执行前必须生成一个 plan 文件
    生成后必须停止，等待你批准
    未经批准不得执行任何修改
---

### 3.4 `.ai/modify/` 或 `.ai/rfc/` — 变更记录(Change Log)
每次实现功能后，AI 必须生成一个变更文件，例如：

.ai/modify/change-20260309-1.md

内容包括：
    变更原因（对应 plan）
    实际修改内容（diff 或 patch）
    影响范围
    风险评估
    是否需要更新 design 文档
    是否需要你进一步 review

AI 的义务：
    所有修改必须在此记录
    diff 必须完整、可读
    若修改与设计不一致，必须提出 RFC
---

### 3.5 `.ai/review/` — Review 与测试结果
AI 完成修改后必须生成：
.ai/review/review-20260309-1.md

内容包括：
    自我 review（Self-review）
    测试结果（手动测试或自动测试）
    是否符合 design
    是否符合 conventions
    是否需要进一步修复

AI 的义务：
    review 必须真实、严格
    若发现问题必须提出修复计划（回到 plan 阶段）

---

## 4. 全流程执行顺序（AI 必须遵守）

## Step 1 — 文档更新（必须等待批准）
AI 必须：

- 更新 `.ai/design` 或 `.ai/plan`
- 停止并等待你的批准

你批准后才能进入下一步。

---

## Step 2 — 实施（Implementation）
AI 执行代码修改：

- 创建文件
- 修改代码
- 重构
- 添加测试

完成后进入下一步。

---

## Step 3 — 记录变更（Change Log）
AI 必须：

- 在 `.ai/modify` 中生成变更记录
- 包含完整 diff

---

## Step 4 — Review & Test
AI 必须：

- 在 `.ai/review` 中生成 review 文档
- 包含测试结果
- 若发现问题 → 回到 Step 1

---

# 5. 用户与 AI 的交互协议（Approval Contract）

AI 必须遵守以下规则：

- **未经你批准不得执行任何修改**
- **每次任务必须先生成 plan**
- **每次设计变更必须先更新 design 文档**
- **每次实现后必须生成 modify + review**

你可以通过一句话批准：

```
批准执行
```

或拒绝：

```
需要修改计划
```

---

# 本协议为项目级别的长期工作契约，AI 必须在整个项目周期内遵守。