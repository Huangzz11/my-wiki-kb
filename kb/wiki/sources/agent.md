---
title: "Agent 智能体"
type: source
tags: [ai-agent, learning]
date: 2026-09-09
source_file: kb/raw/agent.md
last_updated: 2026-09-09
---

# Source: Agent 智能体

## Summary

Agent 是把"流程控制权"交给大模型的系统：给定目标，由 [[LLM]] 在循环中动态决定每一步（读什么、调什么工具、何时停）。与 Workflow 的界线在"谁决定下一步"——代码还是模型。学术源头可追溯至 ReAct 模式（推理与行动交替）。

## Key Claims

- Agent 循环 = 推理 → 行动 → 观察 → 再推理，直到满足停止条件。
- 组成：增强型 LLM（模型 + 工具 + 检索 + 记忆）+ Agent 循环。
- 适用开放、路径不可预知的任务；固定流程任务用 Workflow 更划算。
- 成本与错误率随步骤累积，需要护栏（guardrail）。

## Connections

- 依赖 [[LLM]] 作决策大脑；每一步都读写 [[Context]]（工作台）。
- 用 [[Skill]] 获得"这类活怎么干"的方法（渐进式披露加载）。
- [[Anthropic]] 的《Building Effective Agents》是其权威参考文献。
- 概念关系详见 [[sources/concept-relationship]]。
