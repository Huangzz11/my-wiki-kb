---
title: "LLM（大语言模型）"
type: concept
tags: [llm]
sources: [sources/llm-model, sources/llm-context, sources/agent]
last_updated: 2026-09-09
---

# Concept: LLM（大语言模型）

读过海量文本、靠"预测下一个词"练出来的巨型神经网络：[[Agent]] 的决策大脑。

**三大支柱**
- Transformer + 自注意力（2017，Vaswani 等）
- 大规模预训练（next-token prediction）
- 规模扩展 → 涌现能力（上下文学习 / 指令遵循 / 思维链），再经对齐调优（SFT/RLHF）成为可用助手

**能力边界**：知识截止、幻觉、[[Context]] 窗口有限、只会输出文字。装上工具/检索/记忆（增强型 LLM）并放入 Agent 循环才具备行动力。

供应商：[[OpenAI]]、[[Anthropic]] 等。权威综述：arXiv:2303.18223。
