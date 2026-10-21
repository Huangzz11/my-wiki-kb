---
title: "MultiAgentTeam（多 agent 团队）"
type: concept
tags: [ai-agent, multi-agent, orchestration]
sources: [sources/multi-agent-teams, sources/multi-agent-candidates-github]
last_updated: 2026-10-21
---

# Concept: MultiAgentTeam（多 agent 团队）

几个各自带工具、各自有独立上下文的 [[Agent]] 通过消息协作，把超出单窗口容量、或天然可并行的任务拆开做。它是一套分工机制，跟"一个 agent 装很多工具"的分界线在于上下文是否隔离。

**四个零件**：编排层（拆活派活）、子 agent（独立上下文 + 专属工具）、通信协议（传什么、什么格式）、共享状态（谁持有唯一事实源）。缺任何一个都会出现典型故障：编排层乱派、子 agent 重复劳动、字段漂移、两边状态打架。

**三种生产拓扑**：
- 编排-执行（orchestrator-worker）：一层路由 + 多层并行，可控性最好，生产默认选项。
- 层级式（supervisor/hierarchical）：能做逐分支预算管控与干净审计链，代价是每层一次串行调用，延迟叠加。
- 对等/蜂群（swarm）：并行度最高，可追溯性最差，强监管领域通常不认。

**性能的真实来源**：[[Anthropic]] 的公开数据里，主编 Opus 4 + 子 agent Sonnet 4 比单用 Opus 4 高 90.2%，但 BrowseComp 上光 token 用量一项就解释 80% 的性能方差。多 agent 强，很大程度是因为它在同一问题上花了更多算力（约为普通对话的 15 倍，单 agent 约 4 倍）。

**它的边界**：等思考 token 预算下，单 agent 在多跳推理上稳定打平或超过多 agent（Tran & Kiela, arXiv 2604.02460，三个模型家族对照）。原因是 agent 之间每次转交都只能丢信息、不能加信息。所以它适合上下文碎片化型任务，推理深度型任务用它没好处。

**失败模式**：MAST（arXiv 2503.13657）从 7 个框架、1600+ 轨迹里归纳出 14 种，分三类——规范与系统设计、agent 间错位、任务验证与终止。说明问题多在结构层，调提示词治不了根。

**在 [[WorkBuddy]] 里怎么落地**：它自带四层——[[Skill]]（方法论，注入主对话）、subagent（独立上下文与工具白名单的执行体）、team（多具名 agent 协作）、automation（只管触发时机）。日常的"分头干 + 汇总"用这套就够，不必外接第三方。缝合点是 Skill 的 `context: fork`，它会切断对话历史，依赖当前对话输入的活儿不该走 fork。

**第三方选型**：详见 [[sources/multi-agent-candidates-github]]。真正符合"完整产品 + 原生 MCP + 高星"的只有 [[Ruflo]]（78k，坑多）和 PAL MCP（12k，偏开发场景）两个；其余高星项目是平台或框架，装不进工作台。

相关：[[Agent]]（单体的定义）、[[Context]]（拆分的根本动因是窗口有限）、[[LLM]]（分工后的决策单元）、[[Ruflo]]（一次具体的产品实践）。
