---
name: hotspot-case-card
description: 新传热点案例图卡与笔记生成器。当用户给出一个热点事件并要求「做新传视角分析 + 产出小红书图片」时使用。流程：核对事实（≥2 个独立来源）→ 写 4-5 点理论解析 → 生成 3-6 张 1080×1440 竖版图卡 → 归档到 kb/新传热点事件解读/。支持绿色系卡片与宝厂黑白长图文两套版式（后者可加首页居中刊头）。
agent_created: true
---

# Hotspot Case Card · 新传热点案例图卡与笔记

把一次热点事件做成「图卡 + 笔记」的标准化产出，并归档进本仓库的 `kb/新传热点事件解读/`。

## 触发

用户给出热点事件（有时附参考链接/截图），要求做传播学分析并产出图片。

## 固定产出

1. 一页笔记 `kb/新传热点事件解读/<编号>-<事件名>.md`，结构固定：
   **事件回顾 → 新传视角解析（5 个理论点）→ 考点金句 → 同源案例 → 小红书文案 → 资料来源 → 待核项**
2. 一套 3–4 张图卡 `kb/新传热点事件解读/images/<编号>-<事件名>-0X.png`，**1080×1440（3:4）**

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

图卡 = 固定尺寸的 HTML 卡片 → Playwright 截图。工作目录里建 `<事件名>_新传案例/`，含 `p1.html ~ p4.html`、`style.css`、`shoot.js`。

**两套版式，按用户指定或参考图选定，不要混用：**

| 版式 | 样式基线 | 特点 | 页数 |
|---|---|---|---|
| A. 绿色系卡片 | **`西贝倒闭传闻_新传案例/style.css`**（主色 #12a45c，白底；早期基线 `葫芦娃爷爷_新传案例/style.css` 同款，18 期起补了 `.src-title`/`.sources`/`.disclaim`） | 页头居中大标题 + 短横线、关键词 chips、粗体小标题解析条目、绿色左边框金句块、圆角胶囊标签 | 3–4 张 |
| B. 宝厂黑白长图文 | **`恩施假村官_新传案例/style.css`**（白底 #fff，纯黑白，含 .masthead 刊头样式；旧版基线 `一点点_新传案例/style.css` 无刊头） | 超粗黑 900 字重标题 + 12px 粗下划线、段落式正文、粗边框「理论抓手」框、左右分栏页脚 | 5–6 张 |

无指定时默认沿用该系列**最近一期**的版式，并在动笔前确认一次。
版式沿革：编号 12–14 用 A；15–17 用 B；**编号 18 起用户要求换回 A（绿版），后续默认用 A，除非另有指定**。

**切版式只需两步**（两套样式不同，绝不混用）：把 A 版式样板的 `style.css` 与 `shoot.js` 复制进新目录 → 改 `shoot.js` 里的 `DIR` 和 `pages`。A 版式不含 `.page`/`.masthead`/`.theory`，B 版式的刊头规则不适用于 A。

**A 版式首屏（p1）主标题必须压成一行**。`.case-title` 默认 50px，中文长标题会折成两行并压出 `.title-underline` 的下边界，观感很乱。超长标题按需降到 44–46px（`.num` 同步降 6px），同时把 `letter-spacing` 设为 0。改完必须重新渲染核对。

**A 版式页数按内容定，不要硬套 3 张**。实测 3 张时 p2/p3 常剩约 400px 留白（约占页高 1/4）。视角数 ≥5 或内容较厚时直接出 4 张：p1 = 事件回顾 + 关键词；p2 = 视角 1–2；p3 = 视角 3–5；p4 = 考点金句 + 对照案例 + 参考资料（小字号）+ 待核说明。

**A 版式末页的参考资料**用小字号（`.src-title` 30px 标题 + `.sources` 20px 正文，灰色 #7d8b93），下方可加 `.disclaim` 浅绿底块写待核说明。这套样式已内置在 `西贝倒闭传闻_新传案例/style.css` 末尾，新目录直接继承。

**B 版式首页刊头（可选，用户指定时必加）**

用户要求「首页上面标题居中写 XX」时，在 p1 的 `.page` 内、`.kicker` 之前插入：

```html
<div class="masthead">九月热点事件案例分析</div>
```

样式已内置在 `恩施假村官_新传案例/style.css`。**统一用反白通栏版（黑底白字）**——用户 2026-09-18 明确要求「刊头要醒目一些」：

```css
/* 反白通栏刊头：黑白长图里对比最强，一眼抓住视线 */
.masthead {
  text-align: center;
  font-size: 46px; font-weight: 900; line-height: 1;
  letter-spacing: 15px; text-indent: 15px;   /* text-indent 抵消字距造成的右偏 */
  color: #fff; background: #111;
  padding: 20px 0 22px;
  margin: -8px -18px 22px;   /* 负 margin 让黑条左右出血，比正文宽 → 通栏感 */
}
```

早期版本是 31px + 上下 4px 细线（白底黑字），观感偏弱，已弃用。反白版实际高度约 88px，比旧版多占约 21px；p1 正文底边留 30px 余量即可过 `shoot.js` 的 1372 检测线。

刊头文案按**批次**命名（如「九月热点事件案例分析」），同一批次的图卡共用同一个刊头。加了刊头后 p1 正文相应减一段或删掉收束句。`.masthead` 只在 p1 出现，改这段 CSS 不会波及后续页面。

**页脚规范（B 版式，所有新图沿用）**

- 左侧署名固定为 **「小鱼学新传」**（用户品牌名）。
- 右侧页码格式为 **`N / 总数`**（如 `1 / 5`），五页即 1/5、2/5、3/5、4/5、5/5。
- 不再使用「BAOCHANG · 宝厂」或「新传热点案例 · 15」这类旧署名与编号。

**图片素材**：默认不使用实拍图片，全部纯文字版式（规避版权风险）。用户明确提供合规素材时才加图位。

- 渲染命令（Chromium 已缓存在 `~/AppData/Local/ms-playwright`）：

```bash
cd C:/Users/lancy/.workbuddy/binaries/node/workspace && \
NODE_PATH=C:/Users/lancy/.workbuddy/binaries/node/workspace/node_modules \
C:/Users/lancy/.workbuddy/binaries/node/versions/22.22.2-2/node.exe "<shoot.js 绝对路径>"
```

**溢出校验是必须的**：shoot.js 里 evaluate 读取 `document.documentElement.scrollHeight`，必须 == 1440。首次渲染几乎必然溢出，按以下顺序压缩：卡片 padding → 正文字号/行高 → 各段 margin-top → 精简文案。**溢出超过 100px 时不要硬压，直接拆页**（B 版式一页放 1–2 个理论点 + 1 个理论框最舒服）。

踩过的坑：

- flex column 容器里的 `inline-flex` 元素会被拉伸到满宽，必须加 `align-self: flex-start`。
- 关键词 chips 数量按 6 个 4 字词设计，字号 24.5px / padding 10px 19px 时单行放得下。
- **拆页时不要用字符串切割重建 HTML**。曾因切割后覆写导致原页面内容丢失。正确做法：先 `cp p3.html p4.html` 备份，再逐页编辑。
- **拆页后务必检查 `<head>`**。手工拼接的页面容易丢掉 `<link rel="stylesheet" href="style.css">`，症状是截图变成无样式的纯文本流。
- **标题不要出现单字孤行**。用 `<br>` 主动控制断行，或把标题改短。
- **`overflow: hidden` 会掩盖溢出**，scrollHeight 只提示超了，看不到哪个元素越界。别靠反复试字号，直接写诊断脚本用 `getBoundingClientRect()` 逐个子元素打印 `top/bottom`，一眼定位重叠元素。示例：

```javascript
const info = await page.evaluate(() => {
  const page = document.querySelector('.page');
  return [...page.children].map(el => {
    const r = el.getBoundingClientRect();
    return { cls: el.className, top: Math.round(r.top), bottom: Math.round(r.bottom) };
  });
});
```

重点是检查正文块底边与页脚（`.foot`）顶边是否重叠——页脚是 `position: absolute; bottom: 40px`，正文块一旦越过 1368px 就会压上去。
- **溢出超过 100px 时，减内容比压字号有效**。B 版式一页的信息上限约等于：标题 + 3 段正文 + 1 个理论框。超了就拆页或删次要小节。
- **不要用 heredoc 写 shoot.js**。`cat > shoot.js <<'EOF'` 会吞掉反斜杠：`.replace(/\/g, '/')` 落盘变成 `/\/g`，直接 SyntaxError。改用 Write 工具写 JS 文件，或在脚本里用 `const BS = String.fromCharCode(92)` 再 `.split(BS).join('/')`。
- **用字符串替换往正文里插段落，必然会弄坏 DOM 结构**。`<p>` 常被插到 `</div>` 之外，多出一个闭合标签。替换后必须立刻 `sed -n` 或 Read 检查目标区段，确认新 `<p>` 在 `<div class="body-text">` 内部。
- **先做一轮全局收紧，再逐页处理**。统一收 `.body-text` 行高（1.82→1.74）、段距（20→16px）、`.sec` 外边距（26/16→20/13）、`.theory` 内边距（20/24→17/22），一轮下来多页直接达标，只剩少数页要动文字。逐页调字号又慢又容易反复。
- **`overflow: hidden` 会掩盖溢出**，scrollHeight 只提示超了，看不到哪个元素越界。别靠反复试字号，直接写诊断脚本用 `getBoundingClientRect()` 逐个子元素打印 `top/bottom`，一眼定位重叠元素。示例：

```javascript
const info = await page.evaluate(() => {
  const page = document.querySelector('.page');
  return [...page.children].map(el => {
    const r = el.getBoundingClientRect();
    return { cls: el.className, top: Math.round(r.top), bottom: Math.round(r.bottom) };
  });
});
```

重点是检查正文块底边与页脚（`.foot`）顶边是否重叠——页脚是 `position: absolute; bottom: 40px`，正文块一旦越过 1368px 就会压上去。
- B 版式的页面留白较多，若某页下半部分空白超过 1/4，补一个「延伸」「小结」小节，不要空着。

**A 版式**分工按页数定。3 张时：p1 = 封面 + 关键词 + 事件回顾 + 3 个数据卡；p2 = 解析 1–3；p3 = 解析 4–5 + 考点金句 + 同源案例。4 张时（推荐，见上文）：p1 = 事件回顾 + 关键词；p2 = 视角 1–2；p3 = 视角 3–5；p4 = 考点金句 + 对照案例 + 参考资料 + 待核说明。

**B 版式**六张分工（编号 16 起标准，内容少可退到 5 张）：p1 = 刊头（如指定）+ 编号条 + 主标题 + 事件回顾 + 视角导览框 + 收束句；p2 ~ p5 = 每个视角独占一页，含正文 + 理论抓手框（+ 延伸 / 同类案例）；p6 = 考点金句 + 多视角串联 + 治理动向 + 参考资料（小字号）。

**页码**：写 `N / 总数`，页数变了要同步回改全部页面（本轮 5→6 时用脚本批量替换 `<span>N / 5</span>`）。

**定页数的方法**：先写完全部内容（几个理论点 + 考点金句 + 同源案例 + 参考资料），再按「一页一个视角 + 一个理论框」切分，最后补收尾页。**不要先定页数再往里塞内容**——硬塞的后果是末页被截断，金句和来源都没位置。

**参考资料页排版**：B 版式用 `.sources`，字号 17px、行高 1.55、色值 #7a7a7a，必须加 `word-break: break-all`（长链接会撑破容器）；A 版式用 `.sources`（20px / #7d8b93）。参考资料固定放最后一页，字号小于正文，符合用户要求。

**A 版式的溢出校验**：`shoot.js` 检测 `card.scrollHeight > 1440`，并额外打印 `footTop` 与 `contentBottom`。**当 `contentBottom` 超过 `footTop` 时，即使 `scrollHeight` 仍是 1440，也已经真的压住了页脚**——A 版式 `.foot` 用了 `margin-top:auto`，会吸收掉溢出而不撑高卡片，只靠 scrollHeight 判断会漏检。这是 A 版式相比 B 版式最容易踩的坑。

## 第三步补充：抓取 SPA 类参考资料

参考材料常来自 FlowUs 等单页应用，`WebFetch` 与 `curl` 都只能拿到空壳 HTML。用 Playwright 渲染后取 `document.body.innerText`：

```javascript
const { chromium } = require('playwright');
const b = await chromium.launch();
const p = await b.newPage();
await p.goto(URL, { waitUntil: 'domcontentloaded', timeout: 60000 });
await p.waitForTimeout(8000);          // SPA 渲染要等
const txt = await p.evaluate(() => document.body.innerText);
fs.writeFileSync('输出路径.txt', txt, 'utf8');
```

**脚本必须放在 `C:/Users/lancy/.workbuddy/binaries/node/workspace/` 目录内**，放 `/tmp` 会因模块解析路径问题报 MODULE_NOT_FOUND。

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
