---
title: "大语言模型 LLM"
type: source
tags: [llm, learning]
date: 2026-09-09
source_file: kb/raw/llm-model.md
last_updated: 2026-09-09
---

# Source: 大语言模型 LLM

## Summary

LLM 是"读了海量文字、靠预测下一个词练出来的巨型神经网络"。靠 Transformer 架构 + 大规模预训练获得基础能力，规模跨过门槛后涌现出上下文学习、指令遵循、思维链等小模型没有的能力，再经对齐调优（SFT/RLHF）成为可用助手。在 Agent 系统里它是"只会输出的决策大脑"。

## Key Claims

- 三大支柱：Transformer 自注意力（2017）、预训练（next-token prediction）、规模扩展带来涌现。
- 预训练阶段只会续写；"会对话"来自指令微调与人类反馈对齐。
- LLM 本身只有文字进出，装上工具/检索/记忆（增强型 LLM）并放入 Agent 循环才有行动力。
- 边界：知识截止、幻觉、上下文窗口有限、小任务杀鸡用牛刀。

## Connections

- 是 [[Agent]] 的大脑、[[Context]] 的载体；[[Skill]] 的方法要由它执行。
- 供应商包括 [[OpenAI]]（GPT 系列）、[[Anthropic]]（Claude 系列）等。
- 与上下文、Agent 的机制关系见 [[sources/concept-relationship]]。
- 权威综述：Wayne Xin Zhao 等《A Survey of Large Language Models》（arXiv:2303.18223）。
