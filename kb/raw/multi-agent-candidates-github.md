---
topic: GitHub 上可装进 WorkBuddy 的多 agent 协作产品
mode: research
generated_by: topic-research
date: 2026-10-21
review_status: 数据已实测，结论待人工决策
---

# GitHub 高星多 agent 协作产品筛查

## 判定标准

WorkBuddy 能吃进来的东西只有四条路：MCP server（stdio 或 HTTP，写进 `~/.workbuddy/mcp.json`）、Bash 能调的 CLI、Skill（方法论，非产品）、官方连接器。

所以"能安装在 WorkBuddy 上"的硬标准是：**这个项目自己暴露 MCP server 或 CLI 入口**。一个 Python 框架（CrewAI、MetaGPT、LangGraph 这类）不满足，它需要你写代码把它包成 MCP，成本远超收益。

## 星数实测（2026-10-21，GitHub API / shields.io）

### 第一档：原生 MCP，装完就能用

| 项目 | 星 | 形态 | 维护状态 | 判定 |
|---|---|---|---|---|
| **ruvnet/ruflo**（原 claude-flow，2026 年改的名） | 77,874 | 多 agent swarm 编排 harness，自带 MCP server | 活跃，2026-10-21 仍有提交 | ✅ 唯一"完整产品 + 原生 MCP + 高星"三合一，但坑很多，见下 |
| **BeehiveInnovations/pal-mcp-server**（原 zen-mcp-server，2025-12 改名） | 约 12k | 纯 MCP server，多模型/多角色协作 + CLI 子代理 | 活跃 | ✅ 风险最低的一个 |
| lastmile-ai/mcp-agent | 8,573 | MCP 原生 agent SDK | ⚠️ 最后提交 2026-01-25，停了 | 不推荐 |

### 第二档：平台型，要自己起服务

| 项目 | 星 | 说明 |
|---|---|---|
| n8n-io/n8n | 208,162 | 工作流自动化平台，AI agent 只是节点，本身不做多 agent 协作 |
| Significant-Gravitas/AutoGPT | 187,678 | 平台，不是团队编排 |
| langgenius/dify | 159,072 | Agent 平台，支持 MCP server，可把应用当工具接出去 |
| langflow-ai/langflow | 155,872 | 可视化编排，偏单流程 |
| OpenHands/OpenHands | 91,416 | 编码 agent 平台，Docker 部署 |
| FlowiseAI/Flowise | 55,511 | ⚠️ 仓库已归档（archived），别选了 |
| coze-dev/coze-studio | 21,735 | 字节 Coze 开源自部署版 |

### 第三档：框架，不是产品（不满足"能装"）

| 项目 | 星 | 最后提交 |
|---|---|---|
| FoundationAgents/MetaGPT | 70,941 | 2026-01-21（半年没动） |
| microsoft/autogen | 61,503 | 2026-10-16 |
| crewAIInc/crewAI | 59,926 | 2026-10-20 |
| langchain-ai/langgraph | 43,602 | 2026-10-21 |
| agno-agi/agno | 42,842 | 2026-10-20 |
| stanfordnlp/dspy | 38,710 | 2026-10-20 |
| OpenBMB/ChatDev | 34,561 | 2026-10-16 |
| agentscope-ai/agentscope | 33,555 | 2026-10-20 |
| openai/openai-agents-python | 30,186 | 2026-10-20 |
| mastra-ai/mastra | 28,962 | 2026-10-21 |
| openai/swarm | 22,065 | 2026-04-15（教学项目，已被 agents SDK 取代） |
| google/adk-python | 21,930 | 2026-10-21 |
| camel-ai/camel | 17,940 | 2026-10-19 |
| microsoft/agent-framework | 14,237 | 2026-10-21（AutoGen 的官方继任者） |
| VoltAgent/voltagent | 10,853 | 2026-10-15 |
| kyegomez/swarms | 7,285 | 2026-10-20 |
| OpenBMB/AgentVerse | 5,158 | 2024-09-09（死了） |

另有两个高星但不算编排产品的：`wshobson/agents` 41,272（Claude Code 插件市场）、`davila7/claude-code-templates` 32,794（CLI 配置工具）。

## ruflo 详评

**它是什么**：TypeScript 写的 agent harness，架在 Claude Code / Codex 之上，提供 swarm 编排、跨会话记忆、跨机器联邦、向量 RAG。原生 MCP server，配置方式是：

```json
{
  "mcpServers": {
    "ruflo": {
      "command": "npx",
      "args": ["-y", "ruflo@latest", "mcp", "start"]
    }
  }
}
```

**必须知道的负面事实**（都有出处，不是传闻）：

1. 2026 年 4 月的独立审计发现 `agent_spawn` 和 task 类工具只在记状态，实际干活的还是 Claude Code 自己的 subagent；项目 README 现在也承认"有计划或 agent 记录，不代表工作发生了"。
2. Issue #640：agent 自报 success 时，约 89% 实际是失败的，缺少声明与验收之间的强制机制。
3. 用 better-sqlite3（同步 API），Node 里会阻塞事件循环，真正的并行工作流跑不起来。
4. `npx ruflo@latest` 现在拿到的是 3.7 alpha（写作时是 3.7.0-alpha.11）。要稳定得钉版本，比如 v3.6.30。
5. 安全历史：早期版本有混淆的 preinstall 脚本（v3.5.40 才移除）、一个 SQL 注入 bug、一个路径穿越 bug（v3.58.1 才修）。
6. 文档碎片化，README / wiki / issue / 插件 README 各说一套，官方安装路径换过。

**在 WorkBuddy 里的实际预期**：它的大部分能力挂在 Claude Code 的 hooks 和 plugin 机制上，WorkBuddy 没有这套钩子，接进来能用的主要是 MCP 工具面的那部分（memory、团队总线、部分编排），打个折扣是必然的。

## pal-mcp-server 详评

**它是什么**：一个纯 MCP server，把多个模型/provider 组合成"一个开发团队"。核心工具：`chat`（要第二意见）、`thinkdeep`（深度推理）、`planner`（拆计划）、`consensus`（多模型辩论出共识）、`codereview`、`precommit`、`debug`、`challenge`（防止模型无脑附和）、`apilookup`。2025-12 新增 `clink`，可以从当前会话里起隔离的 CLI 子代理（比如让 Codex 开一个干净上下文做代码审查，只回传结论）。

**优点**：纯 MCP，没有宿主耦合；装完即用；多角色协作是真的（consensus 会让不同模型互驳）；Apache 2.0。

**缺点**：① 偏开发场景，做文档、调研、运营类任务价值打对折；② README 的配置示例是 bash + uvx 脚本，Windows 原生不友好，官方另开了一页 WSL 安装说明；③ 需要自己准备至少一个 provider 的 API key（OpenRouter / Gemini / OpenAI / Azure / Ollama 任选）。

## 结论

**只有两个候选真的符合"完整产品 + 能装 + 高星"：ruflo（78k）和 pal-mcp-server（12k）。**

- 想低风险尝鲜 → pal-mcp-server。但要接受它是开发场景工具，且 Windows 上得走 WSL。
- 想体验"真·多 agent 蜂群" → ruflo，但必须：钉版本（别用 @latest）、先在废弃仓库里试、装完先跑 `ruflo cleanup --dry-run` 看它要往你电脑里写什么、别对着正在用的项目跑 `init`。

**一个前提提醒**：WorkBuddy 自带 subagent 和团队编排，日常的"分头干 + 汇总"不需要装第三方。真正值得装的场景是：你要的是跨模型的对抗式审查（pal 的 consensus），或者需要几十个 agent 并发跑长任务（ruflo 的 swarm）——这两件事现成的 WorkBuddy 做不了。

## 来源

- GitHub REST API 实测（star / pushed_at / archived），2026-10-21
- shields.io 星数接口（ruflo 78k、zen/pal 12k、Agent-MCP 1.3k）
- ruflo README（MCP 配置原文）、pal-mcp-server README（工具清单与配置示例）
- andrew.ooo《Ruflo Review: The Claude Code Harness Behind 77K Stars》——审计记录、安全历史、版本建议
- notatechguy.com《Claude Code swarm tool Ruflo hits 69,000 GitHub stars》——星数与 issue 量、无官方关联声明
- GitHub Discussion #1666（ruflo）——MCP 工具名不匹配、#640 验收失效、better-sqlite3 阻塞
- sublimecoding.com《Ruflo 深度评测》——alpha 版本节奏、文档碎片化、适用边界

**待核实**：pal-mcp-server 的精确星数（取整为 12k，未拿到精确值，API 限速）；ruflo 官方宣称的 100+ agent 数量未经独立验证。
