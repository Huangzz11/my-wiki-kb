---
title: "多 agent 团队：把大任务拆给几个各有专长的 AI"
type: source
tags: [multi-agent, orchestration]
date: 2026-10-21
source_file: kb/raw/multi-agent-teams.md
---

# Source: 多 agent 团队

## Summary

一份八小节的概念学习笔记，把"多 agent 团队"从定义讲到使用边界。核心判断是它擅长处理上下文碎片化，对推理深度型任务没有增益。结尾一节把这套抽象对到 WorkBuddy 的四层机制上（Skill / subagent / team / automation），并给出"要不要开团队"的三步判别顺序。

## Key Claims

- 多 agent 由四个零件组成：编排层、子 agent、通信协议、共享状态。缺任一个都会出现对应故障。
- 生产里能跑通三种拓扑：编排-执行（默认）、层级式（延迟代价大）、对等蜂群（可追溯性差）。有统计称编排-执行约占 70% 生产部署，此数据出自二手分析，未在论文或官方口径交叉验证。
- [[Anthropic]] 数据：Opus 4 主编 + Sonnet 4 子 agent 比单用 Opus 4 高 90.2%；BrowseComp 上 token 用量解释 80% 性能方差；多 agent 约耗 15 倍 token（单 agent 约 4 倍）。
- 等思考 token 预算下，单 agent 在多跳推理上稳定打平或超过多 agent（Tran & Kiela, arXiv 2604.02460）。论证基于数据处理不等式：agent 之间每次转交只能丢信息。
- MAST（arXiv 2503.13657）归纳 14 种失败模式、3 大类，AppWorld 上某些配置故障率可达 86.7%；干预只能提 14% 完成率，说明问题在结构层。
- 便宜的判别法：把准备派给 4 个 agent 的活交给 1 个 agent，给同样的总 token 预算并要求先做前置分析。打平就别开团队。

## Connections

- 与 [[Agent]] 是层级关系：单 agent 是决策单元，多 agent 是这些单元的组织形式。
- 拆分的根本动因是 [[Context]] 窗口有限，见 [[sources/llm-context]]。
- 在 [[WorkBuddy]] 里的落地形态是 [[Skill]] + subagent + team + automation 四层。
- 失败模式那一节与 [[SelfMaintainingWiki]] 的"防幻觉硬规则"同源：都是对"上游错误成为下游事实"的防御。

## Contradictions

- 拓扑占比 70%、独立 peer 架构 +58% / 集中 supervisor +285% token 开销、企业普遍限制 3—4 个 agent 三条，均只有二手行业分析支撑，raw 文档内已标"待核实"。
- 法律运营团队四 agent 案例（81%→96%、2.3h→22min）来自单一二手博客，未见原始披露。
