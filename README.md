# my-wiki-kb · 个人自维护知识库

基于 [llm-wiki-agent](https://github.com/SamurAIGPT/llm-wiki-agent)（MIT）设计思想，适配 WorkBuddy 的自维护互链知识库。

> 本仓库于 2026-09-09 从 `concept-learning-lab` 独立拆分而来：学习资料留在原仓库，知识库单独成仓，职责更清晰。

## 仓库用途

- **沉淀个人概念与实体知识**：把学习资料、调研报告喂入后，由 AI 自动整理成互相 `[[双链]]` 的结构化页面
- **随取随用的个人 wiki**：通过 query 提问，基于库内已核查内容回答，可沉淀新的综合页
- **可继续喂料迭代**：以后读到新资料，放进 `kb/raw/` 说一句 "ingest" 即可收录

## 目录结构

```
my-wiki-kb/
├── .workbuddy/
│   └── skills/
│       └── wiki-keeper/        # 项目级 Skill（知识库维护器）
│           └── SKILL.md        # ingest / query / lint 工作流定义
├── kb/
│   ├── raw/                    # 输入层：源文档（只读存档，共 6 份）
│   │   ├── agent.md / llm-context.md / skill.md / llm-model.md
│   │   ├── concept-relationship.md
│   │   └── llm-wiki-agent-report.md
│   ├── wiki/                   # 知识层：互链页面（首批 ingest 产出 17 页）
│   │   ├── index.md            # 库入口
│   │   ├── log.md              # 操作日志
│   │   ├── overview.md         # 全库综述
│   │   ├── sources/    ×6      # 每份源一页摘要
│   │   ├── concepts/   ×5      # Agent / Context / Skill / LLM / 自维护wiki
│   │   ├── entities/   ×4      # Anthropic / OpenAI / WorkBuddy / llm-wiki-agent
│   │   └── syntheses/          # 沉淀问答（暂空）
│   └── graph/                  # 图谱（预留）
├── README.md
└── .gitignore
```

## 如何在 WorkBuddy 中调用 wiki-keeper

1. 在 WorkBuddy 中**打开本仓库**（作为当前项目）
2. 说指令即可触发项目级 Skill `wiki-keeper`：

| 指令 | 作用 |
|---|---|
| `ingest kb/raw/xxx.md` | 收录一份新源文档：读源 → 建/更新页面 → 维护双链与 index/log/overview |
| `query：wiki 里怎么说 XX` | 基于库内已核查内容回答，可沉淀为 synthesis 页 |
| `lint` | 检查孤儿页、坏链、缺页实体 |

## 设计要点（借鉴 llm-wiki-agent，已适配 WorkBuddy）

- **raw 只读**：源文档不修改，Agent 只在其上提炼
- **防幻觉硬规则**：坏链只报告、不自动建页；无整段照搬
- **渐进式披露**：index/overview 常驻，细节页按需读
- 页面均带元信息头（来源页、收录日期、review_status），可追溯可核查

## 首批内容来源

首批 6 份源文档来自本人的学习仓库 [concept-learning-lab](https://github.com/Huangzz11/concept-learning-lab)（4 份概念学习资料 + 1 份关系说明 + 1 份 llm-wiki-agent 调研报告），均已人工核查。

## 协议说明

本仓库借鉴了 [SamurAIGPT/llm-wiki-agent](https://github.com/SamurAIGPT/llm-wiki-agent)（MIT 协议）的设计思想与目录结构，源码未直接复制；知识库内容为本人学习沉淀。
