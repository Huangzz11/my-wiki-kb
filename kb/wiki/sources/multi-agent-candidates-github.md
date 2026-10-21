---
title: "GitHub 高星多 agent 协作产品筛查"
type: source
tags: [multi-agent, mcp, github, selection]
date: 2026-10-21
source_file: kb/raw/multi-agent-candidates-github.md
---

# Source: GitHub 高星多 agent 产品筛查

## Summary

一次带硬标准的选型筛查。判定门槛定在"项目自己暴露 MCP server 或 CLI 入口"，因为 [[WorkBuddy]] 只能通过 MCP、CLI、Skill、连接器四条路接入外部能力。30 多个候选仓库的星数与最后提交时间于 2026-10-21 实测，按"原生 MCP / 平台型 / 框架"三档列出，逐条给排除理由。

## Key Claims

- 同时满足"完整产品 + 原生 MCP + 高星"的只有两个：[[Ruflo]]（77,874 星，配置 `npx -y ruflo@latest mcp start`）与 BeehiveInnovations/pal-mcp-server（约 12k，纯 MCP，工具含 chat/thinkdeep/planner/consensus/codereview/challenge/clink）。
- pal-mcp-server 偏开发场景，配置示例是 bash + uvx，Windows 需 WSL；原名 zen-mcp-server，2025-12 改名。
- 高星但装不进或不该装的：n8n 208,162（工作流平台）、Dify 159,072（平台，支持 MCP）、MetaGPT 70,941（框架，2026-01 后停更）、AutoGen 61,503、CrewAI 59,926、LangGraph 43,602 —— 全是需要写代码包装的框架。
- Flowise 55,511 星但仓库已 archived，直接排除。
- lastmile-ai/mcp-agent 8,573 星，最后提交 2026-01-25，已停更。OpenBMB/AgentVerse 5,158 星，2024 年后无提交。
- 结论建议：日常"分头干 + 汇总"用工作台自带的 subagent 与 team 即可；只有跨模型对抗式审查、几十个 agent 并发跑长任务这两类需求才值得外接。

## Connections

- 这是 [[MultiAgentTeam]] 的选型落地篇，抽象与产品的对应关系在这里。
- [[Ruflo]] 单独立页，因为它同时是候选也是反面教材。
- 接入方式受 [[WorkBuddy]] 的 MCP 机制约束，见 [[entities/WorkBuddy]]。

## Contradictions

- pal-mcp-server 的精确星数未取到（GitHub API 限速，退回 shields.io 只得到取整值 12k）。
- Ruflo 官方宣称的"100+ agent"未获独立验证，与审计发现的"多数工具只记状态"存在张力，已在 [[Ruflo]] 页并列记录。
