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

## 核查型 Ingest（源材料存疑时）

触发：源文档是图片扫描件、转载合集、历年资料拼盘，或用户明确要求"不许编、来源给链接、搜不到就留空"。

在标准 Ingest 工作流之上追加：

1. **先建核对台账**：为源里每个可独立核对的最小单元（如一年的试题、一份文件）记录三件事——原始来源怎么写的、外部来源怎么写的、判定结论。
2. **交叉验证优先于转录**：每个单元找 ≥1 个独立来源比对；只有一个来源时，标注"单来源待核"，不要写成已确认。
3. **年份 / 日期 / 编号类字段重点查**：这类字段最容易被转载者串位。判定时优先用**结构特征**（如大纲改版后题型结构变化）、其次用多来源一致性，不要只凭出现顺序推断。发现有串位时，把判定依据逐条写进文档，方便他人复核。
4. **留白不填补**：源里缺失或残缺的部分，明确写"缺什么、缺到什么程度"。外部来源能补的，补入并用独立标记（如〔补｜来源编号〕）注明来路；补不齐的，直接留空。
5. **冲突公开化**：源与外部来源表述冲突时，两方原文都保留，不擅自取舍；同时在源页的 `## Contradictions` 段落列出。
6. **同一份材料只建一页**：不要按年份或按科目拆成多页 sources，页面会碎。一份材料对应一个 sources 页，年份与科目差异在页内分节。
7. **raw 存档保留原貌**：转录版与源 PDF 的差异（修错、补入、留白）在 sources 页和文档开头的"勘误清单"里说清楚，raw 文件本身保持一份可读的完整文本。

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
- [ ] 核查型 ingest：每个单元有校对状态标记；缺失处明确留白；冲突两方原文都保留
- [ ] sources 页与 raw 文件一一对应，未按年份/科目拆碎
- [ ] 提醒用户核查后提交 git

## 提交与推送（本仓库特有）

仓库 `Huangzz11/my-wiki-kb` 挂着两条自动推送链（每日 08:00 热点候选、22:30 考研测验），
手工提交前**必须先 fetch + rebase**，否则 push 会被拒。

```bash
git config user.name Huangzz11
git config user.email Huangzz11@users.noreply.github.com
git add -A && git commit -m "..."
git fetch origin main
git rebase origin/main      # 有冲突就解
git push origin main
```

- 远端走 SSH over 443：`ssh://git@ssh.github.com:443/Huangzz11/my-wiki-kb.git`（大陆直连 HTTPS 不稳）。
- **冲突高发文件**：`kb/wiki/index.md`、`kb/wiki/log.md`。两边都是「追加」性质，
  冲突时必须**逐块合并、两方都保留**，禁止用 `--ours` / `--theirs` 整文件覆盖。
- rebase 后仍被拒，多半是远端又多了定时任务提交，再 `git fetch` 一次即可。

### 仓库受损时的恢复（已实际发生过一次）

症状：`.git/refs/` 消失、`git log` 报 not a git repository、对象库缺失（多由中断的 rebase 引起）。

```bash
# 1. 先确认工作区文件仍在（笔记、图卡、元数据文件都只是普通文件，不受 .git 损坏影响）
# 2. 从远端重新克隆一份干净仓库
git clone ssh://git@ssh.github.com:443/Huangzz11/my-wiki-kb.git _kb-fresh
# 3. 把工作区里多出来的新文件复制进 _kb-fresh，并逐一 diff
#    index.md / log.md / README.md / 热点候选目录，确认没有遗漏任何本地改动
# 4. 在 _kb-fresh 里提交并 push
```

**若原目录或其子目录被进程占用、无法整体 `mv` 改名**：不要强行删除，改为**只替换 `.git`**：

```bash
mv .git .git-broken-YYYYMMDD
cp -r ../_kb-fresh/.git .git
git checkout -- .        # 把工作区对齐到 HEAD
```

占用者通常是残留的定时任务进程。若子目录改名失败而 `.git` 改名成功，说明锁在子目录上，替换法可用。

## 边界与注意事项

- `kb/raw/` 只读；`kb/wiki/` 全权归本 Skill 维护。
- 知识库价值在"沉淀 + 互链"，页数少时避免过度建页（一个概念只建一页，宁缺毋滥）。
- 本 Skill 借鉴 MIT 协议开源项目 llm-wiki-agent 的设计；文档记录于 kb/raw/llm-wiki-agent-report.md。
