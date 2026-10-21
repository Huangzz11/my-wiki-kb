---
title: "Context（大模型的上下文）"
type: concept
tags: [llm]
sources: [sources/llm-context, sources/concept-relationship, sources/multi-agent-teams]
last_updated: 2026-10-21
---

# Concept: Context（大模型的上下文）

[[LLM]] 一次生成时眼前能看到的全部信息：系统提示、对话历史、检索资料、工具结果，以 token 放进有限窗口。

**核心推论**
- 窗口即"现场记忆"：被挤出窗口的内容等于没发生过。
- 上下文 ≠ 训练知识：前者来自输入，后者在参数里。
- 中间迷失：长窗口低段利用率是常态。

**对 [[Agent]] 的意义**：上下文是 Agent 的工作台，信息质量决定决策质量；[[Skill]] 通过渐进式披露控制对它的占用。

**窗口有限这件事，是 [[MultiAgentTeam]] 存在的根本理由**：几个 agent 各开一个窗口并行啃，把结果压成少量关键 token 交回主编，本质是用隔离换容量。代价同样落在这里——每次窗口之间的转交都要做摘要，摘要必有损耗，所以拆得越碎、信息损失越多。
