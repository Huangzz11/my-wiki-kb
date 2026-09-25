---
name: hotspot-case-card
description: 【资料库】新传热点案例图卡的技术细节与踩坑记录（版式 CSS 基线、溢出诊断、FlowUs 抓取、历史风格沿革）。日常产出请优先用用户级 skill `xiaohuang-hotspot-case`，本 skill 作为其技术补充查阅。
agent_created: true
---

> **优先使用 `xiaohuang-hotspot-case`**（用户级 skill，位于 `~/.workbuddy/skills/`）。
> 那份是端到端流程（核查 → 五视角 → 图卡 → 文案 → 归档推送 → 桌面副本），日常一句话产出走它。
> **2026-09-24 起它的默认模板已换成 Anthropic 风格版**（暖米白底 + 赭橙强调 + 衬线标题），
> 并在 `templates/` 目录内自带可直接复制的 `style.css` / `shoot.js` / 五页骨架 / 官方字体；
> 绿版降为备选（`templates/legacy-green/`）。
> 本文件保留的是**技术细节与踩坑记录**：版式基线对照、溢出诊断方法、SPA 抓取脚本、各期风格沿革。两者冲突时以 `xiaohuang-hotspot-case` 为准。

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

**A 版式首屏页头与提示语（用户 2026-09-21 明确要求，成为默认）**

- `.page-title` 只写「`M月`新传热点案例」，**不带「（上）」「（下）」「（一）」等批次后缀**。页头是跨期共用的栏目名，不承担分期功能。
- 底部 `.hint` 固定写「**右滑查看 →** ……」（小红书为左右切图，不是上下滑动）。视角数量要与实际页数一致，别写错。
- 该规则对所有版式通用。

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

**署名规范（所有版式通用，2026-09-24 统一改定）**

- 画面署名一律为 **「小鱼学新传」**，**绝不出现「小黄」**（那只是口语昵称，仅内部使用）。
- **期号不上画面**：封面标题不带期号，内页导航条不带期号前缀；期号只留在笔记 frontmatter 与页脚页码。
- 带刊头的版式**每一张左上角都要有署名**：p1 放 `.masthead` 左侧（右侧配日期 `2026.09`，`justify-content: space-between`），p2–p5 放 `.mini-bar` 最左侧，后接事件名与分区名，用 `<span class="sep">/</span>` 分隔。
- 刊头文案按**批次**命名（如「九月新传热点案例分析」），同一批次共用；事件标题作为次级标题，字号要明显小于刊头（Anthropic 版：刊头 62px / 事件标题 56px）。

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
- **A 版式同样适用「留白超 1/4 就补内容」，但补写必须一次到位**。A 版式一页放 2 个视角时，每视角默认只写 2 段往往只到 1000px 左右（页脚线 1347，留白约 341px，视觉上明显发空）。正确做法是每个视角写满 3 段（约 250–300 字）。**分两轮补写会导致第二次直接压住页脚**（20 期实测：一次补两段后 p3 底边 1349 == footTop 1349，页脚被顶下去）。补写后必须重新渲染核对；若超了，**合并相邻两段比删除更安全**——既能压高度，又不丢事实。
- **参考材料原文需谨慎对待**。20 期素材里出现两处问题：一是「按摩乳 / 飞机场」两段文案被混写成乱码，二是引用的罚款金额未标明来源。处理原则：**乱码段落不复述，未二次核实的数字不采用**（改用「多次被行政处罚」这类定性表述），并在「待核项」里说明未采用的原因。反过来，核查时要主动补原文没有的硬数据——20 期补到的「海河案 45 万元罚款落在经销商而非品牌方」正是「违法成本」那一节的论证支点。

**A 版式 5 张页的分工（20 期标准，视角 ≥5 时用）**

p1 = 页头 + pill + 主标题 + chips + 事件回顾（3 段）；p2 = 视角 1–2（各 3 段）；p3 = 视角 3–4（各 3 段）；p4 = 视角 5 + 考点金句；p5 = 同源案例对照（`.ext`，3–4 条）+ 参考资料（`.sources`）+ 待核说明（`.disclaim`）。实测终稿底边 1329 / 1304 / 1246 / 1240 / 1147，均可通过。

**行文风格按用户逐期指定，有两个已用过的档位**

| 档位 | 特征 | 用过 |
|---|---|---|
| **评论体** | 概念化冒号标题（「一杯豆浆的黑丝：……」）、具名学者加年份、结构层落点、「更值得玩味的是」类转折、金句收束 | 编号 20 |
| **论述题答题体** | 视角小标题为「理论名：论点句」、正文按「论点—理论阐释—例证—小结」推进、术语密度高、论断句收束、金句可直接誊抄进答案 | 编号 21 |

不指定时沿用上一期。风格设定写进笔记 frontmatter 下方的引用行，并在 README 末尾注明。

**论述题答题体会显著增加溢出概率**（21 期首轮实测 p2 超 54px、p3 超 145px、p4 超 39px，而 20 期首轮全部通过）。预留策略：写的时候每视角控制在 3 段、每段 3 行以内；渲染后优先「精简文字」而不是「压缩字号」。

**p5 是溢出重灾区，必须预先控量**（23 期首轮实测 p5 超 237px，占该轮全部超量的 74%——同轮 p1/p3/p4 合计仅超 88px）。该页堆了 `.ext` 同源案例 + `.sources` 参考资料 + `.disclaim` 待核说明三块，任一超量都会连锁顶爆。写 p5 时的硬上限：

| 块 | 上限 |
|---|---|
| `.ext` 同源案例 | 4 条，每条 ≤ 60 字（压成一句，删掉评述尾巴） |
| `.sources` 参考资料 | 9 条（超了就合并同日同源的多篇报道） |
| `.disclaim` 待核说明 | 4 条，每条 ≤ 50 字 |

溢出时的删减顺序：**先压 `.ext` 每条 → 再合并 `.sources` → 最后精简 `.disclaim`**。参考资料是可信度背书，不要为腾地方先删它。

**单页微调字号用页面内联 style，不要改共享的 `style.css`**。某个页面溢出而其他页正常时，在该页 `<head>` 里 `<link>` 之后插一个 `<style>` 块覆盖：

```html
<style>
  .sec-title { margin-top: 20px; }
  .point { margin-top: 19px; }
  .point-body { font-size: 25.5px; line-height: 1.68; margin-top: 10px; }
</style>
```

这样只影响该页，其余页面和后几期复制的模板不受污染。21 期 p3、p4 均用此法通过（底边从 1532、1426 回落到 1295、1301）。

**用户说「语言风格参照参考材料原文」时怎么做**

20 期用户明确要求语言风格也参照链接原文。做法：先提取原文风格特征（概念化冒号标题、具名学者加年份、结构层落点、「更值得玩味的是」类转折、金句收束的评论体），据此改写图卡的视角小标题与正文句式，**把每视角从 2 段加厚到 3 段**以匹配原文的概念密度；同时在笔记 frontmatter 下加一行「语言风格」说明、在 README 末尾注明风格切换，方便后续几期保持一致。

**A 版式**分工按页数定。3 张时：p1 = 封面 + 关键词 + 事件回顾 + 3 个数据卡；p2 = 解析 1–3；p3 = 解析 4–5 + 考点金句 + 同源案例。4 张时（推荐，见上文）：p1 = 事件回顾 + 关键词；p2 = 视角 1–2；p3 = 视角 3–5；p4 = 考点金句 + 对照案例 + 参考资料 + 待核说明。

**B 版式**六张分工（编号 16 起标准，内容少可退到 5 张）：p1 = 刊头（如指定）+ 编号条 + 主标题 + 事件回顾 + 视角导览框 + 收束句；p2 ~ p5 = 每个视角独占一页，含正文 + 理论抓手框（+ 延伸 / 同类案例）；p6 = 考点金句 + 多视角串联 + 治理动向 + 参考资料（小字号）。

**页码**：写 `N / 总数`，页数变了要同步回改全部页面（本轮 5→6 时用脚本批量替换 `<span>N / 5</span>`）。

**定页数的方法**：先写完全部内容（几个理论点 + 考点金句 + 同源案例 + 参考资料），再按「一页一个视角 + 一个理论框」切分，最后补收尾页。**不要先定页数再往里塞内容**——硬塞的后果是末页被截断，金句和来源都没位置。

**参考资料页排版**：B 版式用 `.sources`，字号 17px、行高 1.55、色值 #7a7a7a，必须加 `word-break: break-all`（长链接会撑破容器）；A 版式用 `.sources`（20px / #7d8b93）。参考资料固定放最后一页，字号小于正文，符合用户要求。

**A 版式的溢出校验**：`shoot.js` 检测 `card.scrollHeight > 1440`，并额外打印 `footTop` 与 `contentBottom`。**当 `contentBottom` 超过 `footTop` 时，即使 `scrollHeight` 仍是 1440，也已经真的压住了页脚**——A 版式 `.foot` 用了 `margin-top:auto`，会吸收掉溢出而不撑高卡片，只靠 scrollHeight 判断会漏检。这是 A 版式相比 B 版式最容易踩的坑。

**第二个盲区：`contentBottom` 本身可能是恒定值**（2026-09-24 做 Anthropic 版封面大标题时撞上）。若版式里 `.hint` 也带 `margin-top:auto`，`contentBottom` 会**恒等于 `footTop − hint 与 foot 的固定间距`**（Anthropic 版恒为 1333），内容再怎么涨都不变，只盯它必然漏检。可靠判据是另外两个：

- **`hintGap`** = `hint.getBoundingClientRect().top − 前一元素.bottom`。**落到 0 就是真挤满**，再长 1px 即溢出；正常应留 30px 以上。
- **`miniBarW`** = `.mini-bar` 的 `scrollWidth`。**> 928**（1080 − 76×2）说明内页导航条已折行。

```javascript
const hint = document.querySelector('.hint');
const prev = hint.previousElementSibling;
const gap = prev ? Math.round(hint.getBoundingClientRect().top - prev.getBoundingClientRect().bottom) : null;
```

新做版式时，这两个判据要一并写进 `shoot.js`。

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
   - **平铺，不建子目录。** 12—22 期全部平铺在 `images/`，不要建 `<编号>-<事件名>_assets/`。（`_assets/` 子目录是 `kb/考研每日反馈/` 的规则，两个目录约定不同，别串。）23 期首次归档误用子目录，已纠正。
   - **不另建独立文案文件。** 小红书文案写进笔记 `<编号>-<事件名>.md` 的 `## 小红书文案` 章节。
2. 更新 `kb/新传热点事件解读/README.md` 的目录表。
3. 复制一套到桌面 `C:\Users\lancy\Desktop\<编号>_<事件名>_图卡\`，**只放 5 张 PNG**（20、22 期均为纯 PNG，不放文案与笔记）。
4. 更新 `kb/wiki/index.md` 的「热点解读」段、追加 `kb/wiki/log.md`。
5. 提交推送：

```bash
# 必须带 SSH 保活参数（不带时大陆网络下 push 常报 send-pack 断连）
# 必须加 `| cat` 管道——用 out=$(git push ...) 命令替换会静默失败（变量空、无报错、实际未推）
GIT_SSH_COMMAND="ssh -i ~/.ssh/id_ed25519 -o IdentitiesOnly=yes \
  -o ServerAliveInterval=15 -o ServerAliveCountMax=10 -o TCPKeepAlive=yes -o ConnectTimeout=30" \
  git push origin main 2>&1 | cat
# 成功判据用 SHA 比对，不看输出文本
git fetch origin main >/dev/null 2>&1
[ "$(git rev-parse HEAD)" = "$(git rev-parse origin/main)" ] && echo "VERIFIED" || echo "WARN: 未推送成功"
```

- 需 `dangerouslyDisableSandbox: true`，默认沙箱拒读 `~/.ssh/` 会报 `Connection reset ... port 443`。
- 推送前先 `git fetch` + rebase（仓库有两条自动推送链，远端常领先）。

- 图卡源文件（HTML/CSS/JS）**不提交**，只提交 PNG 与笔记。
- 仓库 `git config user.name` 需为 `Huangzz11`、`user.email` 为 `Huangzz11@users.noreply.github.com`。

**小红书文案必须与图卡严格对齐**

交付文案前核对一件事：**正文里列的视角数量、顺序、名称，必须与图卡完全一致**。19 期（正文写 4 个视角、图卡 5 张）、20 期（正文写「四个传播学视角」、图卡 5 个视角）、21 期（正文写「四个论述角度」、图卡 5 个视角）**连续三期栽在同一处**。用户对数字很敏感，看到「四个视角」配五张图会直接质疑严谨性。修法：写文案时直接从图卡的 `.point-head` 抄视角名，不要凭记忆重写。

用命令核验，不要靠眼睛：

```bash
# 正文视角数（应为 5）
grep -c "^① \|^② \|^③ \|^④ \|^⑤ " <笔记路径>
# 图卡视角总数（各页 class="idx" 之和，应为 5）
grep -c 'class="idx"' <图卡目录>/p*.html
```

另：文案里不写未经核实的传播表现（如「上了热搜」），拿不准就改成中性表述（「引发争议」）。

## 落盘前自检

- [ ] 事实有 ≥2 个来源；冲突与单来源项进了「待核项」
- [ ] 5 个理论点都具名
- [ ] 无「不是……而是……」句式（含变体）
- [ ] 图卡实测 1080×1440，无裁切
- [ ] README 目录表、index、log 三处同步
