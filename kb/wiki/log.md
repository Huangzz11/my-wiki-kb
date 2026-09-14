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

## [2026-09-14] ingest | 云南大学新传专硕 334/440 真题（2020-2026）

来源：用户提供的图片型 PDF《【云南大学】2020-2026年真题合集【秃头研究所】》（18 页）。逐页识读转录为可复制文字版，存档于 kb/raw/yunnan-mjc-334-440-2020-2026.md。

处理要点：

- 转录 14 套试题（7 年 × 334/440），水印层未保留。
- 逐套与启航教育、Free考研、豆尖班、云上新传、征诚、觅游、云大新传考研等来源交叉比对。
- 修正原始 PDF 两处年份标题错误：标为 2024 年 334 的实为 2023 年 334，标为 2023 年 334 的实为 2022 年 334。依据是 334 大纲 2023 年改版（取消选择题、增设名词解释）与多来源年份标注一致。
- 缺失与残缺全部留白标注，未做推断填补。

产出：[[sources/yunnan-mjc-334-440-past-papers]]、实体页 [[YunnanUniversity]]、综合页 [[syntheses/yunnan-mjc-kaoqing-analysis]]；index 与 overview 同步更新。

待办：2020、2021 年选择题内容待补；2023 年 440 待第二个来源比对；2022 年 440 选择题第 11 题题干残缺待补；2026 年 334 材料分析第 2 题需确认原文。
