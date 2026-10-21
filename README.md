# my-wiki-kb · 个人自维护知识库

基于 [llm-wiki-agent](https://github.com/SamurAIGPT/llm-wiki-agent)（MIT）设计思想，适配 WorkBuddy 的自维护互链知识库。

> 本仓库于 2026-09-09 从 `concept-learning-lab` 独立拆分而来：学习资料留在原仓库，知识库单独成仓，职责更清晰。

## 仓库用途

- **沉淀个人概念与实体知识**：把学习资料、调研报告喂入后，由 AI 自动整理成互相 `[[双链]]` 的结构化页面
- **随取随用的个人 wiki**：通过 query 提问，基于库内已核查内容回答，可沉淀新的综合页
- **可继续喂料迭代**：以后读到新资料，放进 `kb/raw/` 说一句 "ingest" 即可收录
- **新传热点事件解读专题**：考研热点案例库，每期一页笔记 + 一套小红书规格图卡，见 `kb/新传热点事件解读/`（编号 12—34）
- **新传高频名词专题**：纯理论名词图卡，讲清定义、命题、评价与配套概念，见 `kb/新传高频名词/`（7 个名词，含「新传名词解释 100 讲」连载）
- **每日测验与反馈**：每晚一份综合卷 HTML（20 题，含错题回滚），配套学习反馈笔记，见 `kb/考研每日反馈/`
- **热点候选侦察**：每日扫榜 + 硬门槛筛选，产出候选清单供人工选定，见 `kb/热点候选/`

## 目录结构

```
my-wiki-kb/
├── .workbuddy/
│   └── skills/
│       ├── wiki-keeper/        # 项目级 Skill（知识库维护器）
│       │   └── SKILL.md        # ingest / query / lint 工作流定义
│       ├── hotspot-case-card/  # 项目级 Skill（热点案例图卡生成器）
│       │   └── SKILL.md        # 事实核查 → 5 点理论解析 → 1080×1440 图卡 → 归档
│       └── hotspot-scout/      # 项目级 Skill（热点候选侦察器）
│           └── SKILL.md        # 扫榜 → 硬门槛筛选 → 每日候选清单（不产图卡）
├── kb/
│   ├── raw/                    # 输入层：源文档（只读存档，共 8 份）
│   │   ├── agent.md / llm-context.md / skill.md / llm-model.md
│   │   ├── concept-relationship.md
│   │   ├── llm-wiki-agent-report.md
│   │   ├── yunnan-mjc-334-440-2020-2026.md
│   │   └── multi-agent-team.md
│   ├── wiki/                   # 知识层：互链页面（共 24 页）
│   │   ├── index.md            # 库入口
│   │   ├── log.md              # 操作日志（唯一记录入口）
│   │   ├── overview.md         # 全库综述
│   │   ├── sources/    ×8      # 每份源一页摘要
│   │   ├── concepts/   ×6      # Agent / Context / Skill / LLM / 自维护wiki / 多agent团队
│   │   ├── entities/   ×6      # Anthropic / OpenAI / WorkBuddy / llm-wiki-agent / 云南大学 / Cognition
│   │   └── syntheses/  ×1      # 沉淀问答（云南大学新传专硕考情分析）
│   ├── 新传热点事件解读/        # 专题：新传考研热点案例分析（编号 12—34，共 23 期）
│   │   ├── README.md           # 专题收录规则、版式沿革与目录表
│   │   └── images/             # 配套图卡 1080×1440（3:4），共 116 张
│   ├── 新传高频名词/            # 专题：纯理论名词图卡（7 个名词，含 100 讲系列）
│   │   ├── README.md           # 命名规范、版式说明与已收录表
│   │   └── <名词名>/            # 平铺 PNG，命名 <名词名>-0X.png
│   ├── 热点候选/                # 每日热点候选清单（YYYY-MM-DD.md，共 25 份）
│   └── 考研每日反馈/            # 每日学习反馈（2026-09-09 起）＋ 测验/ 综合卷 HTML
├── README.md
└── .gitignore
```

> **本地工作目录不进仓库**：`<编号>_<事件名>_图卡/`、`<名词名>_名词卡/` 是图卡制作时的 HTML/CSS/截图工作区，只在本地存在，已在 `.gitignore` 中按顶层目录忽略，只提交成品 PNG。
>
> **已移除**：`kb/graph/` 自 2026-09-09 建库起一直为空，图谱功能未落地，2026-10-21 连同本节原「图谱（预留）」一行一并删除。



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

## 内容来源

首批 6 份源文档来自本人的学习仓库 [concept-learning-lab](https://github.com/Huangzz11/concept-learning-lab)（4 份概念学习资料 + 1 份关系说明 + 1 份 llm-wiki-agent 调研报告），均已人工核查。

2026-09-14 增补第 7 份：`yunnan-mjc-334-440-2020-2026.md`（云南大学新传专硕 334/440 真题 2020—2026），据此产出实体页 `云南大学` 与综合页 `云南大学新传专硕考情分析`。

其余内容（热点解读、名词卡、每日测验与反馈）为本人原创整理，每期笔记内标注信源与口径说明，不来自外部资料库的直接搬运。

## 协议说明

本仓库借鉴了 [SamurAIGPT/llm-wiki-agent](https://github.com/SamurAIGPT/llm-wiki-agent)（MIT 协议）的设计思想与目录结构，源码未直接复制；知识库内容为本人学习沉淀。
