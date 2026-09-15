---
name: hotspot-case-card
description: 新传热点案例图卡与笔记生成器。当用户给出一个热点事件并要求「做新传视角分析 + 产出小红书图片」时使用。流程：核对事实（≥2 个独立来源）→ 写 5 点理论解析 → 生成 3 张 1080×1440 竖版图卡 → 归档到 kb/新传热点事件解读/。
agent_created: true
---

# Hotspot Case Card · 新传热点案例图卡与笔记

把一次热点事件做成「图卡 + 笔记」的标准化产出，并归档进本仓库的 `kb/新传热点事件解读/`。

## 触发

用户给出热点事件（有时附参考链接/截图），要求做传播学分析并产出图片。

## 固定产出

1. 一页笔记 `kb/新传热点事件解读/<编号>-<事件名>.md`，结构固定：
   **事件回顾 → 新传视角解析（5 个理论点）→ 考点金句 → 同源案例 → 小红书文案 → 资料来源 → 待核项**
2. 一套 3 张图卡 `kb/新传热点事件解读/images/<编号>-<事件名>-0X.png`，**1080×1440（3:4）**

编号在目录 README 的表格里顺延，不跳号。

## 第一步：事实核查（不可跳过）

- 至少 2 个独立来源交叉核对；只有一个来源的细节，写进「待核项」并标注**单来源待核**。
- 来源之间冲突时两方表述都保留，不擅自取舍。
- 重点核这三类字段：**事发日期、涉事主体名称、具体数字**。
- 常见媒体错误要主动纠正。已踩过的坑：把 K 字头普速列车写成「高铁」；把走红日当成事发日。
- 查不到就留白，禁止编造。

## 第二步：写解析

- 5 个理论点，每个必须落到**具名的学者或理论**上（如哈布瓦赫集体记忆、戈夫曼拟剧理论、恩特曼框架理论、汤普森新可见性、涂尔干失范）。
- 第 5 点尽量给一个**论断**，而不是复述常识。
- 每个理论点 150–230 字，控制在图卡里占 5–7 行。
- 金句要能直接抄进考场答案。
- 禁止「不是……而是……」句式（含「并非…而是…」「不在于…而在于…」变体）。

## 第三步：出图卡（技术方案）

图卡 = 固定尺寸的 HTML 卡片 → Playwright 截图。工作目录里建 `<事件名>_新传案例/`，含 `p1.html ~ p3.html`、`style.css`、`shoot.js`。

- 样式基线：复制 `葫芦娃爷爷_新传案例/style.css`（绿色系 #12a45c，白底，1080×1440 卡片）。
- 渲染命令（Chromium 已缓存在 `~/AppData/Local/ms-playwright`）：

```bash
cd C:/Users/lancy/.workbuddy/binaries/node/workspace && \
NODE_PATH=C:/Users/lancy/.workbuddy/binaries/node/workspace/node_modules \
C:/Users/lancy/.workbuddy/binaries/node/versions/22.22.2-2/node.exe "<shoot.js 绝对路径>"
```

**溢出校验是必须的**：`shoot.js` 里 evaluate 读取 `.card` 的 `scrollHeight`，必须 == 1440，`footBottom` ≤ 1440。首次渲染几乎必然溢出，按以下顺序压缩：卡片 padding → 正文字号/行高 → 各段 margin-top → 精简文案。

踩过的坑：

- flex column 容器里的 `inline-flex` 元素会被拉伸到满宽，必须加 `align-self: flex-start`。
- 关键词 chips 数量按 6 个 4 字词设计，字号 24.5px / padding 10px 19px 时单行放得下。

三张分工：p1 = 封面 + 关键词 + 事件回顾 + 3 个数据卡；p2 = 解析 1–3；p3 = 解析 4–5 + 考点金句 + 同源案例。

## 第四步：归档与提交

1. 图卡从 `<事件名>_新传案例/images/` 复制到 `kb/新传热点事件解读/images/` 并重命名为 `<编号>-<事件名>-0X.png`。
2. 更新 `kb/新传热点事件解读/README.md` 的目录表。
3. 更新 `kb/wiki/index.md` 的「热点解读」段、追加 `kb/wiki/log.md`。
4. 提交推送：

```bash
git add -A && git commit -m "feat(热点解读): 收录 <编号> <事件名>" && \
git push origin main    # remote 为 ssh://git@ssh.github.com:443/Huangzz11/my-wiki-kb.git
```

- 图卡源文件（HTML/CSS/JS）**不提交**，只提交 PNG 与笔记。
- 仓库 `git config user.name` 需为 `Huangzz11`、`user.email` 为 `Huangzz11@users.noreply.github.com`。

## 落盘前自检

- [ ] 事实有 ≥2 个来源；冲突与单来源项进了「待核项」
- [ ] 5 个理论点都具名
- [ ] 无「不是……而是……」句式（含变体）
- [ ] 图卡实测 1080×1440，无裁切
- [ ] README 目录表、index、log 三处同步
