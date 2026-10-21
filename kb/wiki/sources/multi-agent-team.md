---
title: "多 Agent 团队（Multi-Agent Team）调研"
type: source
tags: [multi-agent, ai-agent, engineering]
date: 2026-10-21
source_file: kb/raw/multi-agent-team.md
last_updated: 2026-10-21
---

# Source: 多 Agent 团队调研

## Summary

一份 2026-10 的调研，主题是多个 agent 组成团队协作的机制、收益条件与失效边界。核心结论可以用一句话概括：团队解决的是上下文容量与并行时延，收益来自协作结构；能并行的部分是"读"，必须单线程的部分是"写"。同期收集了 2025—2026 年最重要的两派立场（Anthropic 的工程实践与 Cognition 的反对意见）以及它们后来达成的和解。

## Key Claims

- 四部件构成一个团队：团队领导（拆解、派活、汇总）、成员（各自独立上下文）、共享任务板（认领 + 依赖）、信箱（成员间直连，可绕开领导）。
- 成员不继承领导的对话历史（WorkBuddy 官方口径），所以任务背景必须写进初始 prompt，写不全成员就会猜。
- **读出并发，写入串行**——这是判断要不要拆团队的第一判据。并行读者增加智能，并行写者制造冲突。
- Anthropic 的 Research 系统用 orchestrator-worker：Opus 4 当 lead、Sonnet 4 当子代理，内部评测比单 Opus 4 高 90.2%，同时消耗约 15 倍 token（单 agent 约 4 倍）。
- 性能归因上，token 用量单独解释 80% 的方差（BrowseComp），工具调用次数与模型选择补齐到 95%。多 agent 主要靠"花掉更多 token 去想"取胜。
- Cognition 的《Don't Build Multi-Agents》（2025-06-12）给出两条上下文工程原则：共享上下文与完整轨迹；动作携带隐性决策。
- Cognition 十个月后自我修正（2026-04）：写在单线程时多 agent 可行，能跑的形状是 map-reduce-and-manage；用干净上下文的独立审查者效果更好。
- MAST（Cemri 等，2025）用 7 个框架、1600+ 条轨迹归纳 14 种失败模式，分系统设计、成员间错位、任务验证缺位三类；摘要把"性能增益往往很小"写成结论。
- MetaGPT 把人类 SOP 编码进提示序列，用装配线角色分工与结构化中间产物（PRD、设计文档）压制级联幻觉，2023 年的 SOTA 路线。
- A2A（Google 提出，2025-06 捐给 Linux Foundation）管 agent 与 agent 之间的通信，与管工具的 MCP 互补。
- **同预算下的反证**：Tran 与 Kiela（arXiv:2604.02460，2026-04）用数据处理不等式论证，固定推理 token 预算且上下文利用充分时，单 agent 的信息效率更高；在三个模型家族上，等预算下单 agent 在多跳推理上稳定打平或超过多 agent。多 agent 的优势大部分可以用"没算账的算力"解释。

## Connections

- 上层概念是 [[Agent]]：多 agent 团队是 Agent 在"多实例协作"方向上的扩展，控制权从单个模型分散到 lead + 成员。
- 每个成员各带独立 [[Context]]，团队的价值一半来自把工作记忆切成互不污染的几块；另一半来自并行省下的墙钟时间。
- 成员的方法论靠 [[Skill]] 加载，[[WorkBuddy]] 的成员会读项目级 `.workbuddy/skills/`，这一点与单 agent 一致。
- [[Anthropic]] 提供正面工程证据（Research 系统），本库新增 [[Cognition]] 页记录反方与后续修正。
- 与 [[Skill]] 所代表的"方法固化"互补：Skill 解决"这类活怎么干"，团队解决"这么多活谁在什么时候干"。

## Contradictions

- **2025-06 的两篇同周文章表面冲突**：Anthropic 报 90.2% 提升，Cognition 建议别建多 agent。两者不构成事实矛盾，负载类型不同（检索以读为主、编码以写为主）。本库的处理方式：并列保留两方原文数字与结论，用读/写比例作为解释变量，不裁决谁对。
- **多 agent 是否提升基准表现**：Anthropic 内部评测显著提升，MAST 观察到流行基准上增益往往很小。差异来自评测对象（自家检索任务 vs 跨框架公开基准），已在概念页标注口径。
- 待核实项（详见 raw 文档末节）：MetaGPT 的 HumanEval 85.9% / MBPP 87.7% 未在 arXiv 摘要中出现；Cognition 复查篇确切日期原文只写"10 个月前"；Claude Code 子代理 20 个并发上限来自二手博客。
