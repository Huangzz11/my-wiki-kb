---
title: "llm-wiki-agent 开源项目调研报告"
type: source
tags: [research, pkm, opensource]
date: 2026-09-09
source_file: kb/raw/llm-wiki-agent-report.md
last_updated: 2026-09-09
---

# Source: llm-wiki-agent 开源项目调研报告

## Summary

对开源项目 SamurAIGPT/llm-wiki-agent（约 3.5k star、MIT）的机制拆解：一个让编码 Agent 自动维护个人知识库的"技能包"。核心是"文件即接口"——raw/ 只读输入，wiki/ 由 Agent 维护的结构化互链知识层，graph/ 图谱；CLAUDE.md/AGENTS.md/GEMINI.md 是同一份 schema 给不同 Agent 的方言。本知识库（kb/）即借鉴其范式。

## Key Claims

- 五条工作流：ingest / query / health（零 LLM 预检）/ lint / graph，分层控制成本。
- 反幻觉硬规则：坏链只报告不自动建页；自动链接需 promote gate；页面需 ≥2 出链。
- 与 RAG 差异：摄入时编译一次持续更新，对"每次查询临时检索"。
- 借鉴价值：方法论文件化、上下文预算管理（index/overview 常驻）、先免费后付费、防幻觉内建流程。

## Connections

- 是 [[SelfMaintainingWiki]] 范式与 [[LLMWikiAgent]] 项目的直接资料。
- 与 [[Skill]] 思路同源（方法论写进文件让 Agent 执行），跑在 [[Agent]] 之上。
- 使用 [[LLM]]（Claude/Codex/Gemini 等）驱动，与本库运行环境 [[WorkBuddy]] 形成对照。
