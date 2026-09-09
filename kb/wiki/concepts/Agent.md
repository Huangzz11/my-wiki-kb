---
title: "Agent（智能体）"
type: concept
tags: [ai-agent]
sources: [sources/agent, sources/concept-relationship, sources/llm-wiki-agent-report]
last_updated: 2026-09-09
---

# Concept: Agent（智能体）

把流程控制权交给 [[LLM]] 的系统：给目标，模型在循环中动态决定下一步（规划 → 调用工具 → 观察 → 再规划），直到完成或触发停止条件。

**与 Workflow 的界线**：流程控制权在代码（工作流）还是在模型（Agent）。

**组成**：增强型 LLM（模型 + 工具 + 检索 + 记忆）+ Agent 循环。

**何时用**：开放、路径不可预知的任务；固定流程任务用更简单方案。

相关：[[Context]]（决策工作台）、[[Skill]]（方法包）、[[LLM]]（大脑）、[[SelfMaintainingWiki]]（Agent 驱动的知识管理实践）。
