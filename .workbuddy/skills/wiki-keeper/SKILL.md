---
name: wiki-keeper
description: 自维护互链知识库维护器（self-maintaining wiki）。当用户想把资料喂进仓库内 kb/ 知识库（说「ingest / 收录 / 喂进知识库 / 存进 wiki」+ 文件或主题）、想查询知识库内容（说「wiki 里怎么说 XX / 查一下知识库 / 知识库总结」）、或想检查知识库质量（说「lint 知识库 / 检查 wiki」）时使用。设计借鉴开源项目 SamurAIGPT/llm-wiki-agent：raw/ 只读输入、wiki/ 为结构化知识层、页面互链、index/log/overview 自动维护。接收任意源文档或主题，持续复用。
agent_created: true
---

# Wiki Keeper · 自维护知识库

## 适用场景

- 用户丢来新资料（Markdown 源、网页内容、论文摘要），要求收进知识库。
- 用户问"知识库里怎么说的 / 帮我总结知识库"。
- 用户要求检查知识库结构、补孤儿页、查矛盾。

仓库内的 `kb/` 就是被本 Skill 维护的知识库：
```
kb/
├── raw/      # 输入层：源文档（md）。可读，勿改
├── wiki/     # 知识层：Agent 唯一负责维护的结构化 wiki
│   ├── index.md       # 全库目录（每次改动必更新）
│   ├── log.md         # 追加式操作日志
│   ├── overview.md    # 跨源活综述（每次 ingest 后修订）
│   ├── sources/       # 每份源文档一页：summary + 关键论断 + 连接
│   ├── entities/      # 实体页：机构、项目、人物（如 Anthropic、llm-wiki-agent）
│   ├── concepts/      # 概念页：核心概念/方法（如 Agent、上下文、Skill、LLM）
│   └── syntheses/     # 沉淀的问答/综合（query 后按需存档）
└── graph/   # 预留：知识图谱数据与可视化（build 脚本）
```

## 页面规范

每页都以 YAML frontmatter 开头，正文用 `[[双链]]` 连接其他页面：

```markdown
---
title: "页面标题"
type: source | entity | concept | synthesis
tags: []
sources: []        # 支撑该页的源文件 slug 列表
last_updated: YYYY-MM-DD
---
```

命名：源页用源文件名 kebab；实体/概念页用 TitleCase（如 `Anthropic.md`、`Agent.md`）；合成页用 kebab。

## Ingest 工作流（核心）

触发：`ingest <文件路径或主题>`。若给主题而非文件，先在仓库内检索对应材料（如 learning-materials/、kb/raw/）。

步骤（按序）：
1. 读源文档全文（md/html 均可，html 先提炼正文文本）。
2. 读 `kb/wiki/index.md` 与 `kb/wiki/overview.md`，掌握现有知识库，避免重复建页。
3. 若源文件不在 `kb/raw/`，先复制一份进去（源=只读存档）。
4. 写/更新 `kb/wiki/sources/<slug>.md`：2-4 句摘要、关键论断清单、与已有页面能建立的 `[[连接]]`。
5. 更新 `kb/wiki/index.md`（Sources 段追加；如有新实体/概念页则同步追加到对应段）。
6. 更新 `kb/wiki/overview.md`：按最新内容修订整体综述，保持跨源综合视角。
7. 新建/更新 `kb/wiki/entities/*.md`：源里反复出现的机构、项目、人物，若无页则建，有则补充。
8. 新建/更新 `kb/wiki/concepts/*.md`：核心概念/方法，同样去重后建或补。
9. 记录矛盾：新源与旧知识冲突时，在相关 sources 页标注"矛盾"并在 log 中记录。
10. 追加 `kb/wiki/log.md`：`## [YYYY-MM-DD] ingest | <标题>`（可附一行摘要）。
11. 校验：检查新建页是否都进了 index，`[[双链]]` 是否指向真实页面；打印本次变更摘要。

### Source 页模板

```markdown
---
title: "..."
type: source
tags: []
date: YYYY-MM-DD
source_file: kb/raw/xxx.md
---
## Summary
## Key Claims
## Connections   # [[概念]] 与 [[实体]] 关系
## Contradictions
```

## Query 工作流

触发：`query: <问题>` / "wiki 里怎么说的"
1. 读 `kb/wiki/index.md` 定位相关页 → 读对应页。
2. 综合回答，引用带 `[[PageName]]`。
3. 问用户是否将回答沉淀为 `kb/wiki/syntheses/<slug>.md`。

## Lint 工作流

触发：`lint` / "检查知识库"
- 孤儿页：无入链的 wiki 页。
- 坏链：指向不存在页面的 `[[链接]]`。
- 缺页实体：被 ≥3 页提及但无专页的实体。
- 陈旧摘要：源更新后未同步的 sources 页。
输出 lint 报告；不要自动删除或自动建页（只报告，改动需用户确认）。

## 自检清单（落盘前过一遍）

- [ ] frontmatter 四字段齐全（title/type/tags/sources/last_updated）
- [ ] 每页有 ≥1 条真实 `[[双链]]` 指向已存在页面
- [ ] index.md 与磁盘实际页面一致
- [ ] log.md 追加了本次操作
- [ ] 无整段照搬源文档（页面是自己组织的摘要与连接）
- [ ] 无「不是……而是……」句式
- [ ] 不编造：页面内容都来自已读源，来源链接保持可核查
- [ ] 提醒用户核查后提交 git

## 边界与注意事项

- `kb/raw/` 只读；`kb/wiki/` 全权归本 Skill 维护。
- 知识库价值在"沉淀 + 互链"，页数少时避免过度建页（一个概念只建一页，宁缺毋滥）。
- 本 Skill 借鉴 MIT 协议开源项目 llm-wiki-agent 的设计；文档记录于 kb/raw/llm-wiki-agent-report.md。
