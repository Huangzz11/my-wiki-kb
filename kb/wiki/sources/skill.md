---
title: "Skill（Agent Skill）"
type: source
tags: [skill, learning]
date: 2026-09-09
source_file: kb/raw/skill.md
last_updated: 2026-09-09
---

# Source: Skill（Agent Skill）

## Summary

Skill 把"做某类任务的方法论"固化成文件（SKILL.md：YAML 元数据 + 步骤），放在规定目录，让 [[Agent]] 按需加载执行。它是 Agent 跨会话的程序性记忆，也是团队经验的版本化存档。本知识库本身由仓库内的 wiki-keeper 项目级 Skill 驱动，就是该机制的活例子。

## Key Claims

- SKILL.md 必含 name/description；description 是触发依据。
- 渐进式披露：元数据常驻、完整正文触发才读、references 按需加载，多个 Skill 不撑爆 [[Context]]。
- 两级存放：用户级 `~/.workbuddy/skills/`（个人跨项目）与项目级 `.workbuddy/skills/`（随仓库走、随 Git 分发）。
- Skill ≠ Prompt（一次性文本 vs 可管理资产）；Skill ≠ MCP/工具（方法论 vs 连接通道）。
- 安全风险：恶意 Skill 可诱导 AI 执行危险操作，需审计来源。

## Connections

- 服务对象是 [[Agent]]，加载介质是 [[Context]]。
- 本库的 wiki-keeper、仓库的 concept-learner 都是项目级 Skill 实例，环境为 [[WorkBuddy]]。
- [[Anthropic]] 系（Claude）与 [[OpenAI]] 生态均有 Skill 规范。
- 与 [[Agent]]、[[Context]] 的关系详见 [[sources/concept-relationship]]。
