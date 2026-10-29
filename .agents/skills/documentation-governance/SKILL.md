---
name: documentation-governance
description: Decide, write, or review durable project documentation from verified requirements and implementation. Use when Main assesses documentation impact, Documentation updates approved files, or Review checks architecture documents and navigation.
---

# 正式文档标准

正式文档帮助读者理解和使用当前 Git 分支的项目。以现有实现、有效需求和可定位的证据为依据，记录长期有效的事实。

## 内容与来源

| 内容 | 位置 |
| --- | --- |
| 项目用途、快速开始、文档入口 | 根 `README.md` |
| 阅读顺序、核心文档链接、已有文档目录职责 | `docs/README.md` |
| 当前的开发需求、目标场景、约束与验收要求 | `docs/spec.md` |
| 已实现的架构、模块职责、状态所有权、依赖、关键流程与必要契约 | `docs/map.md` |
| 未实施方案、任务计划、阻塞、执行命令与结果、审查结论 | 当前会话、Issue 或 PR |

各分支沿用相同文档路径，由 Git 保存版本并随代码合并。模板中的占位提示是编写指导，不能作为已实现能力的证据；新项目尚无实现时，在任务材料中讨论设计。

- spec.md 在对应分支开发完成后清空重置到模板状态，开发相关记录交给 git 本身。
- 最底层的繁琐工程细节和开发约束、注意事项等不应该出现在 map.md 和 README 中，而是记录在 spec.md。
- 文档的更新与编写需要严格按照模板格式，更新过程中不能丢失格式。

## 写入与审查

1. 对照 Main 确定的文档范围和事实，读取相关实现与必要验证证据；冲突或缺失证据返回 Main 决策。
2. 将有效事实写入相应文档，直接描述当前状态；替换过时段落，历史通过 Git 查阅。
3. 文档新增、移动、删除或目录职责变化时，同步维护 `docs/README.md` 的链接和说明。只有实际内容需要独立阅读时才建立子目录。
4. 检查受影响链接、源码位置和表述是否与实现一致，避免重复记录同一契约。

正式文档不包含任务进度、更新记录、调试经过、待定设计或完成日志，这些是 git 的任务。架构解释与操作指南按读者问题区分；不预建空目录、历史归档或额外报告。
