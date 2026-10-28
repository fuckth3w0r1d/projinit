# 项目协作入口

## 共同约束

- 使用中文，先理解任务、场景和已有约定；必要信息不足或需要改变已确认需求时向用户提出问题，已授权范围内的常规实现自主推进。
- 优先复用现有代码、成熟方案和已安装依赖，按实际需求决定结构与验证范围。
- 使用 Git 管理共享规则、角色配置、代码和正式文档；Git push 由用户执行。
- 临时材料放在项目内明确指定的位置，任务完成后清理；不为普通修改创建备份或反复执行无关检查。
- 委派范围是各角色的操作边界；技能只提供方法，不改变职责、授权和权限。

## 上下文入口

从 [文档导航](docs/README.md) 定位当前需求、架构和已有指南，再按任务读取相关代码与配置。接入具体项目时，在本文件或相应目录的局部指令中补充实际源码、构建和验证入口。

正式文档采用固定路径，各 Git 分支保存自己的版本，随代码审查和合并。任务计划、验证记录与未实施方案保存在会话、Issue 或 PR 中。

## 角色选择

| 角色 | 规范 | 职责 |
| --- | --- | --- |
| Main | [.agents/roles/main/main.md](.agents/roles/main/main.md) | 默认交互角色，负责需求、设计、委派、文档决策和验收 |
| Coding | [.agents/roles/coding/coding.md](.agents/roles/coding/coding.md) | 实现与自测，返回文档建议和代码证据 |
| Testing | [.agents/roles/testing/testing.md](.agents/roles/testing/testing.md) | 独立验证，修改获准测试文件 |
| Review | [.agents/roles/review/review.md](.agents/roles/review/review.md) | 只读审查，报告问题与依据 |
| Documentation | [.agents/roles/documentation/documentation.md](.agents/roles/documentation/documentation.md) | 按 Main 决策维护指定正式文档和导航 |

加载所属角色规范作为行为指令。需要审计、维护或理解其他角色文件时，可以将其作为分析材料读取；读取不会切换角色或继承对方权限。

`AGENTS.md`、角色规范、技能和 `.codex/` 属于治理配置，应通过明确的配置维护任务修改。正式文档至少包括 `docs/` 和根 `README.md`；项目在其他位置维护的正式说明也纳入文档范围。

按任务选择自身角色目录和 `.agents/skills/` 中的相关技能，仅在适用时读取正文及引用资源。角色 TOML 声明位于 `.codex/agents/`；实际权限取决于父会话和宿主配置，角色文字不构成独立沙箱。
