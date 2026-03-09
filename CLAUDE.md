# Claude Project Guide

本项目使用 AI 协作开发流程。所有 AI 的行为规范、执行顺序、审批机制与文档要求均定义在：

**→ `.ai/conventions/workflow.md`**

AI 在开始任何任务前必须：

1. 阅读并遵守 `.ai/conventions/workflow.md`
2. 按 workflow 中的流程执行：
   - 与用户交流，根据需求先更新设计或计划文档（参考模板路径见 workflow）
   - 停止并等待用户批准（批准仅指允许修改代码）
   - 获批后再进行代码修改
   - 完成后生成变更记录与 review 文档

模板与目录（参考）：
- 设计总览模板：`.ai/design/overview.md`
- 任务计划模板：`.ai/plan/task-TEMPLATE.md`
- 变更记录模板：`.ai/modify/change-TEMPLATE.md`
- Review 模板：`.ai/review/review-TEMPLATE.md`
- RFC 模板（可选）：`.ai/rfc/rfc-TEMPLATE.md`

本文件仅作为入口说明，不包含具体流程内容。

如需了解完整流程，请阅读：

```
.ai/conventions/workflow.md
```

AI 必须严格遵守该文件中的所有规则。
