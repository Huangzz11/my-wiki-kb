---
title: "Cognition"
type: entity
tags: [company, ai-lab, coding-agent]
sources: [sources/multi-agent-team]
last_updated: 2026-10-21
---

# Entity: Cognition

美国 AI 公司，Devin（自主编程 agent）与 Devin Review 的开发方。在 agent 架构讨论里，它的位置是一手反方证据的提供者。

## 两篇关键文章

- **《Don't Build Multi-Agents》（2025-06-12，Walden Yan）**：提出上下文工程的两条原则——共享上下文与完整 agent 轨迹；动作携带隐性决策，冲突的决策带来坏结果。文章用"两个子代理分别做 Flappy Bird 的背景与小鸟，风格对不上"说明并行 writer 的脆弱性，默认推荐单线程线性 agent，长任务用专门的压缩模型处理历史。
- **《Multi-Agents: What's Actually Working》（2026-04）**：十个月后自我修正，不推翻原结论，收窄适用范围——写在单线程、多个 agent 只贡献"智力"而非"动作"时，多 agent 能跑通。落地形态是 map-reduce-and-manage，配合干净上下文的独立审查者（Devin Review 平均每 PR 抓 2 个 bug，约 58% 属严重级别）。

## 与另一派的关系

同周 [[Anthropic]] 发布 Research 系统的工程复盘，报出 90.2% 的提升。两篇并列看并不矛盾：Anthropic 的检索负载以读为主，Cognition 的编码负载以写为主。这条"读/写"分界后来成为 [[MultiAgentTeam]] 概念页的第一判据。

本知识库中，Cognition 的作用是给多 agent 讨论提供反方与修正方的原文位置，避免只引用正面案例。
