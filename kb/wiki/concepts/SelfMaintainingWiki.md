---
title: "Self-Maintaining Wiki（自维护知识库）"
type: concept
tags: [pkm, methodology]
sources: [sources/llm-wiki-agent-report]
last_updated: 2026-09-09
---

# Concept: Self-Maintaining Wiki（自维护知识库）

把"知识管理"本身做成 [[Agent]] 任务的范式：喂入源文档，Agent 按一份 schema（= [[Skill]]）持续维护互链的结构化 wiki，而不是靠人手工整理。

**范式要素**（源自 [[LLMWikiAgent]] 项目，本库 kb/ 即其实例）
- 输入/输出分层：raw/ 只读存档，wiki/ 归 Agent 全权维护。
- 页面互链：`[[双链]]` 构成可导航的知识网络；index/log/overview 由 Agent 自动同步。
- 防幻觉内建：坏链只报告不自动建页；内容更新前先读库内现状（复用 [[Context]] 管理思路）。
- 质量分层：结构检查（廉价）与语义检查（烧 token）分开跑。

**与 PKM 的关系**：把"第二大脑"从收藏夹升级为持续编译、可交叉验证的结构化资产。
