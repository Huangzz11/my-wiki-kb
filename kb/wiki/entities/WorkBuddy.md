---
title: "WorkBuddy"
type: entity
tags: [product, ai-workspace]
sources: [sources/skill, sources/llm-wiki-agent-report, sources/multi-agent-team]
last_updated: 2026-10-21
---

# Entity: WorkBuddy

本项目（concept-learning-lab）所在的 AI 工作台环境，支持用户级 `~/.workbuddy/skills/` 与项目级 `.workbuddy/skills/` 两级 [[Skill]] 机制。

在本知识库中的作用：concept-learner 与 wiki-keeper 两个项目级 Skill 的运行环境；与 [[LLMWikiAgent]] 面向的 Claude Code/Codex/Gemini CLI 生态形成对照——文件驱动的 Agent 范式在 WorkBuddy 中的落地。

**多 agent 能力的两种形态（2026-10 补）**
- **Agent Teams**：主会话当团队领导，生成若干独立成员，共享任务列表（可认领、可设依赖）+ 信箱（成员间直连）。成员各自独立上下文窗口，加载项目上下文但不继承领导的对话历史。产品上还提供委派模式（限制领导只用协调工具）、计划审批、`@成员名` 直连。官方标注为实验特性，已知限制包括单会话只能管一个团队、不支持嵌套团队。
- **自主组队（v2.154.0）**：agent 可自行招募同事、建频道、挂任务板，lead 每轮拿任务板快照批量派活并催办，团队完工后主动回报。护栏为 50 同事 / 20 频道 / 每频道 2—6 人 / 50 任务项 / 创建深度 ≤2，设置里有总开关。

与 [[MultiAgentTeam]] 的关系：WorkBuddy 提供的是"网状 + 任务板"形态的实现，比 [[Anthropic]] 那种放射状 orchestrator-worker 多了成员间横向通信，协调开销也更高。
