---
title: "WorkBuddy"
type: entity
tags: [product, ai-workspace]
sources: [sources/skill, sources/llm-wiki-agent-report, sources/multi-agent-teams, sources/multi-agent-candidates-github]
last_updated: 2026-10-21
---

# Entity: WorkBuddy

本项目（concept-learning-lab）所在的 AI 工作台环境，支持用户级 `~/.workbuddy/skills/` 与项目级 `.workbuddy/skills/` 两级 [[Skill]] 机制。

在本知识库中的作用：concept-learner 与 wiki-keeper 两个项目级 Skill 的运行环境；与 [[LLMWikiAgent]] 面向的 Claude Code/Codex/Gemini CLI 生态形成对照——文件驱动的 Agent 范式在 WorkBuddy 中的落地。

**[[MultiAgentTeam]] 的原生实现是四层**：[[Skill]]（方法论，注入主对话）、subagent（独立上下文与工具白名单的执行体，如 hotspot-renderer、quiz-grader）、team（多具名 agent 经消息与共享任务列表协作）、automation（只负责触发时机，与前三层正交）。扩展外部能力只有四条路：MCP server 配进 `~/.workbuddy/mcp.json`、CLI、Skill、官方连接器——这也是第三方多 agent 产品能否装进来的判定门槛，见 [[sources/multi-agent-candidates-github]]。
