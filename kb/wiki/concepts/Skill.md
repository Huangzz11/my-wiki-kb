---
title: "Skill（Agent Skill）"
type: concept
tags: [skill]
sources: [sources/skill, sources/llm-wiki-agent-report]
last_updated: 2026-09-09
---

# Concept: Skill（Agent Skill）

把"某类任务怎么做"固化成文件（SKILL.md：YAML 元数据 + 步骤 + 可选 resources），让 [[Agent]] 判断相关后按需加载执行。是 Agent 跨会话的程序性记忆，随 Git 版本管理与分发。

**关键机制**
- 渐进式披露：name/description 常驻（数十 token），正文触发才读，references 按需读。
- 两级存放：用户级（个人跨项目）/ 项目级（随仓库走）。

**易混区分**：Skill ≠ 长 Prompt（资产 vs 一次性文本）；Skill ≠ MCP/工具（方法论 vs 连接）。
**风险**：恶意 Skill 可注入危险指令，需审计来源。

本仓库实例：concept-learner（概念学习）、wiki-keeper（知识库维护）。运行于 [[WorkBuddy]]。
