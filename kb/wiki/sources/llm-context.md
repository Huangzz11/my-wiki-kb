---
title: "大模型的上下文（Context）"
type: source
tags: [llm, learning]
date: 2026-09-09
source_file: kb/raw/llm-context.md
last_updated: 2026-09-09
---

# Source: 大模型的上下文

## Summary

上下文 = [[LLM]] 生成当前内容时"眼前能看到的全部信息"，以 token 形式放进有限的上下文窗口（输入 + 输出 + 推理 token 总和上限）。模型没有窗口之外的记忆——它是 Agent 的"现场工作台"。

## Key Claims

- 上下文窗口超限会截断/遗忘；早期内容被挤出窗口后模型"失忆"且不自知。
- 上下文 ≠ 训练知识（参数记忆）：前者来自每次输入，后者来自训练语料。
- 长上下文有"中间迷失"现象：窗口大 ≠ 全都用得好。
- 多轮对话"记住"历史，是因为历史每次被重新塞进输入。

## Connections

- 决定 [[Agent]] 的每次决策质量：窗口里信息错，决策就错。
- [[Skill]] 靠渐进式披露控制对上下文的占用（元数据常驻、正文按需加载）。
- [[LLM]] 是承载上下文的模型本体；官方口径见 [[OpenAI]] 与 [[Anthropic]] 文档。
- 与 [[LLM]]、[[Agent]] 的关系详见 [[sources/concept-relationship]]。
