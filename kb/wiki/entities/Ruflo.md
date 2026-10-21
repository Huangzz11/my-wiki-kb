---
title: "Ruflo（原 Claude Flow）"
type: entity
tags: [multi-agent, mcp, open-source]
sources: [sources/multi-agent-candidates-github]
last_updated: 2026-10-21
---

# Entity: Ruflo（原 Claude Flow）

GitHub `ruvnet/ruflo`，2026 年从 Claude Flow 改名而来。TypeScript 写的 agent harness，架在 Claude Code / Codex 之上，提供 swarm 编排、跨会话记忆、跨机器联邦与向量 RAG。截至 2026-10-21 实测 77,874 星，仍在活跃提交。

**它为什么值得单独建页**：它是目前唯一同时满足"完整的多 agent 协作产品 + 原生 MCP server + 高星"三条件的项目。配置进任何 MCP 客户端只需要：

```json
{"mcpServers": {"ruflo": {"command": "npx", "args": ["-y", "ruflo@latest", "mcp", "start"]}}}
```

**必须同时记住的负面事实**（均有公开出处）：

- 2026 年 4 月的独立审计发现 `agent_spawn` 与 task 类工具只在记录状态，真正干活的是宿主自己的 subagent；项目 README 现在也承认"有计划或 agent 记录，不代表工作发生了"。
- Issue #640：agent 自报 success 时约 89% 实际失败，缺少声明与验收之间的强制机制。
- 依赖 better-sqlite3（同步 API），Node 环境会阻塞事件循环，真正的并行跑不起来。
- `@latest` 当前指向 3.7 alpha，要稳定必须钉版本。
- 安全历史：早期混淆的 preinstall 脚本（v3.5.40 移除）、SQL 注入、路径穿越（v3.58.1 修复）。

**跟本库的关系**：它是 [[MultiAgentTeam]] 这套抽象的一次真实产品化尝试，也是"star 数衡量的是注意力，不衡量能力"的现成例证。接入 [[WorkBuddy]] 时能用的部分主要落在 MCP 工具面——它的大量能力挂在 Claude Code 的 hooks 与 plugin 机制上，那边没有对应钩子，打折是必然的。

判断依据见 [[sources/multi-agent-candidates-github]]（含完整星数实测表与其余候选的排除理由）。
