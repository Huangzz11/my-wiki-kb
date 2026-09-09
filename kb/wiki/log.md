# Wiki Log

> 追加式操作记录。格式：`## [日期] 操作 | 标题`。操作：ingest / query / lint / graph / report。

## [2026-09-09] ingest | 首批 6 份源文档入库

初始化知识库（kb/），由 wiki-keeper Skill 执行。第一批源文档：

- Agent 智能体学习资料 → [[sources/agent]]
- 大模型的上下文学习资料 → [[sources/llm-context]]
- Skill 学习资料 → [[sources/skill]]
- 大语言模型 LLM 学习资料 → [[sources/llm-model]]
- 三概念关系说明 → [[sources/concept-relationship]]
- llm-wiki-agent 开源项目调研报告 → [[sources/llm-wiki-agent-report]]

产出：index、overview、6 个 sources 页、5 个 concepts 页（[[Agent]] [[Context]] [[Skill]] [[LLM]] [[SelfMaintainingWiki]]）、4 个 entities 页（[[Anthropic]] [[OpenAI]] [[WorkBuddy]] [[LLMWikiAgent]]）。

设计说明：本知识库借鉴 MIT 开源项目 llm-wiki-agent 的"raw 只读输入 + wiki 结构化输出 + 页面互链"范式，落地于 WorkBuddy 项目级 Skill（.workbuddy/skills/wiki-keeper），相关分析见 [[sources/llm-wiki-agent-report]]。
