---
name: frontend-slides
description: 从零创建或由 PowerPoint 转换而来的、动画丰富、零依赖的 HTML 演示文稿,默认中文原生,同样支持英文与中英混排。Use when 用户要求"做幻灯片""做 PPT""做演示""做发布会页面""把 PPT 转成网页"。通过视觉预览引导没有设计经验的用户选定审美,而非用抽象词描述。Create stunning, animation-rich HTML presentations from scratch or by converting PowerPoint files. Use when the user wants to build a presentation, convert a PPT/PPTX to web, or create slides for a talk/pitch.
---

# Frontend Slides(中文原生版)

为非设计师生成单文件、零依赖、动画驱动的 HTML 演示文稿。**默认以简体中文为一等公民**:字体堆栈、字号最小值、行高、标点处理、避头尾、首行缩进、密度上限,全部按中文阅读习惯校准。英文内容、中英混排也是支持目标,不再降级处理。

## 核心原则

1. **零依赖** — 单个 HTML 文件,所有 CSS/JS 全部内联。不引 npm,不打包,不上框架。
2. **看图说话(Show, Don't Tell)** — 用视觉预览替代抽象选项。让用户"看到什么就说要什么",而不是逼他们用词描述偏好。
3. **拒绝 AI 流水线感(Anti-AI-Slop)** — 不要紫到蓝渐变 + Inter + 居中卡片那一套。每一份都要像专门为这个内容定制过。
4. **视口贴合,不允许妥协** — 每一张 `.slide` 必须正好等于 100vh,**不允许在单张幻灯片内部出现滚动**。内容超出?就拆成下一张。
5. **中文优先,但不排斥英文** — 默认按中文做字号、行高、避头尾、密度;遇到英文/数字片段自动走拉丁字体回退;混排靠 `font-feature-settings: "palt"` 和合适的空格处理。

## 设计审美总则

LLM 默认会朝"训练分布的中心"收敛,在前端这就是俗称的"AI 流水线感"。避免之:做出有性格、有出处、有上下文的界面。

聚焦四件事:

- **字体**:选有性格的字体。**英文部分**远离 Inter / Roboto / Arial / system-ui 这些过曝面孔,从 Fontshare(Clash Display、Satoshi、General Sans)、Google Fonts(Fraunces、Bodoni Moda、Syne、Cormorant、Outfit、Plus Jakarta Sans)等里挑;**中文部分**远离微软雅黑、宋体直接套用,选思源宋体/思源黑体多 weight 切换、LXGW 霞鹜文楷(书卷气)、得意黑 Smiley Sans(粗壮现代)、ZCOOL 站酷系列(展示性)、HarmonyOS Sans / MiSans(几何现代)、马善政体 / 龙藏体(笔触感)等。
- **配色**:确立"主-辅-强调"三角关系。主色压住面积,强调色只用在勾点,而不是均匀撒开。从 IDE 配色、文化母题(宣纸、霓虹、印刷油墨、地铁导视)取灵感,而不是从模板站。
- **动画**:以高光时刻为主——一次精心编排的入场(`animation-delay` 错峰揭示)永远比满屏微互动更打动人。CSS 能做就别用 JS,Motion 库只在 React 场景考虑。
- **背景**:层叠 CSS 渐变 + 几何图形 + 噪点纹理建立空气感,而不是甩个纯色完事。

需要明确避免的"AI 流水线感"陷阱:

- 字体:Inter / Roboto / Arial / 微软雅黑 / 宋体 当显示字体
- 配色:白底紫渐变、`#6366f1` indigo、彩虹色卡片堆叠
- 版式:一律居中、英雄区 + 三卡片网格、全部 12 列对齐
- 装饰:写实插画、不分场景的玻璃拟态、无意义投影

要敢于做不对称、做强对比、做特定文化语境(港式、宣纸、地铁导视、磁带、报刊)。生成时还容易在 12 套预设里反复挑同一套(尤其 Space Grotesk + 紫色),**自觉地跳出去**。

## 视口贴合硬规则

下面这些是 every slide × every presentation 都成立的不变量:

- 每个 `.slide` 必须有 `height: 100vh; height: 100dvh; overflow: hidden;`
- **所有字号、间距用 `clamp(min, preferred, max)`** — 禁止固定 px / rem
- 内容容器要有 `max-height` 约束
- 图片:`max-height: min(50vh, 400px)`
- 必须包含 700px、600px、500px 三档高度断点
- 必须支持 `prefers-reduced-motion`
- **永远不要直接对 CSS 函数取负**(`-clamp()` / `-min()` / `-max()` 会被浏览器**静默丢弃**,无报错)— 必须写成 `calc(-1 * clamp(...))`
- **中文专属**:正文字号下限不得低于 `clamp(0.875rem, 1.5vw, 1.125rem)`(汉字小于 12px 在常见显示器上严重退化);行高 1.6–1.8;`word-break: normal; overflow-wrap: anywhere;` 兜底;启用 `hanging-punctuation: first allow-end;`

**生成 HTML 前必须读 `viewport-base.css` 并把它整体内联进 `<style>`。**

### 单页内容密度上限

中文比英文密度高约 1.8 倍(单字符承载更多语义)。下表已按中文字符数校准;英文场景按"约等于这么多视觉权重"放宽 ~1.4 倍。

| 幻灯片类型 | 中文上限                                                       | 英文上限(参考)                                |
| ---------- | -------------------------------------------------------------- | ----------------------------------------------- |
| 标题页     | 主标题 ≤ 12 字 + 副标题 ≤ 20 字 + 可选标签                     | 1 heading + 1 subtitle + optional tagline       |
| 内容页     | 1 个标题 + 4–6 条要点(每条 ≤ 15 字) 或 2 段(每段 ≤ 60 字) | 1 heading + 4–6 bullets / 2 paragraphs          |
| 特性网格   | 1 个标题 + 至多 6 张卡片(卡片标题 ≤ 6 字 / 描述 ≤ 24 字)     | 1 heading + 6 cards max(2×3 / 3×2)             |
| 代码页     | 1 个标题 + 8–10 行代码                                         | 1 heading + 8–10 lines of code                  |
| 引言页     | 引言 ≤ 30 字 + 出处                                            | 1 quote(max 3 lines) + attribution             |
| 图片页     | 1 个标题 + 1 张图(图高 ≤ 60vh)                              | 1 heading + 1 image(max 60vh height)           |

**任何一页内容超过上限,就拆成下一张。永不挤压,永不滚动。**

---

## 阶段 0:识别模式

判断用户意图,三选一:

- **模式 A:新建演示** — 从零开始 → 进入阶段 1
- **模式 B:PPT 转换** — 用户给了 `.pptx` 文件 → 进入阶段 4
- **模式 C:增强已有** — 用户给了一份 HTML,想改进它 → 先读懂再改,**严格遵守下面的"模式 C 修改规则"**

### 模式 C:修改规则

改老内容时,最容易破坏的是视口贴合:

1. **加内容之前**:数清现有元素数量,对照密度上限
2. **加图**:必须 `max-height: min(50vh, 400px)`;如果当前页已经满,直接拆成新一页放图
3. **加文字**:超出 4–6 条 / 60 字段落 → 拆成续页
4. **任何修改之后必查**:`.slide` 还有 `overflow: hidden`、新元素全部用 `clamp()`、图片有视口相对高度、内容在 1280×720 下能完整放下
5. **主动重组**:发现改动会导致溢出,主动拆页并告知用户,**不要等用户发现再补救**

---

## 阶段 1:内容采集(新建演示)

**一次性发起 AskUserQuestion**,把所有问题一并问完,不要一问一答拉长对话:

**问题 1 — 用途**(header: `"用途"`)
这份演示给谁看?选项:融资路演 / 教学培训 / 大会演讲 / 内部汇报

**问题 2 — 篇幅**(header: `"篇幅"`)
大概多少页?选项:短(5–10 页) / 中(10–20 页) / 长(20+)

**问题 3 — 内容就绪度**(header: `"内容"`)
你的内容到什么程度了?选项:全部就绪 / 大纲笔记 / 只有主题

**问题 4 — 主要语言**(header: `"语言"`)
内容以什么语言为主?选项:简体中文(推荐) / 中英混排 / 英文为主 / 其他

**问题 5 — 是否需要浏览器内编辑**(header: `"编辑"`)
是否需要在浏览器里直接改文字?选项:
- "需要(推荐)" — 浏览器内点字即可改,自动存 localStorage,可导出
- "不需要" — 只展示,文件更小

**记住语言和编辑两个选项**,它们决定阶段 3 的字体堆栈和是否注入编辑相关代码。

如果用户有内容,请他/她直接贴出来。

### 阶段 1.2:图片评估(若用户提供了图)

如果用户选择"无图片",跳到阶段 2。

如果给了图片目录:

1. **列清单** — 列出所有 `.png .jpg .svg .webp` 等
2. **逐张查看** — 用 Read 工具(Claude 是多模态)
3. **逐张评估** — 是什么内容、是否可用(USABLE / NOT USABLE,给理由)、表达什么概念、主色调
4. **联合构建大纲** — 图片和文字**同步设计**幻灯片结构,而不是"先排版再塞图"(例如 3 张截图 → 3 个特性页;1 个 logo → 标题页 + 收尾页)
5. **AskUserQuestion 确认**(header: `"大纲"`):"这份大纲和选图对吗?" 选项:OK / 想换图 / 想改大纲

**预览中嵌 logo**:若识别出可用 logo,在阶段 2 的 3 个预览里都把它(base64 内联)放上 — 用户能看到自己的品牌在三种风格下分别长什么样。

---

## 阶段 2:审美探索

**这是"看图说话"的核心阶段。**多数人无法用语言描述自己的审美。

### 阶段 2.0:选择路径

问(header: `"风格"`):
- "给我看看选项"(推荐) — 根据情绪生成 3 张预览
- "我自己挑" — 直接从预设列表里选

**自己挑**:列出预设让用户挑(预设定义见 [STYLE_PRESETS.md](STYLE_PRESETS.md)),跳到阶段 3。

### 阶段 2.1:情绪选择(引导发现)

问(header: `"氛围"`, multiSelect: true, 最多 2 项):
希望观众有什么感受?选项:
- **沉稳/可信** — 专业、值得托付
- **激情/振奋** — 创新、大胆、有锋芒
- **平和/聚焦** — 清晰、思辨、不喧哗
- **共鸣/触动** — 有情绪、有记忆点
- **东方/书卷** — 中式、典雅、有笔触(中文场景新增)

### 阶段 2.2:生成 3 张预览

根据情绪 + 主要语言,生成 3 张单页 HTML,展示字体、配色、动画、整体气质。读 [STYLE_PRESETS.md](STYLE_PRESETS.md) 获取预设详细规格(含中文字体配对)。

| 情绪          | 推荐预设(中文场景优先)                                                |
| ------------- | ----------------------------------------------------------------------- |
| 沉稳/可信     | Bold Signal / Electric Studio / Dark Botanical / 杂志感 CN              |
| 激情/振奋     | Creative Voltage / Neon Cyber / Split Pastel / 港式霓虹                 |
| 平和/聚焦     | Notebook Tabs / Paper & Ink / Swiss Modern                              |
| 共鸣/触动     | Dark Botanical / Vintage Editorial / Pastel Geometry                    |
| 东方/书卷     | 宣纸水墨 / Paper & Ink + LXGW WenKai / 杂志感 CN(用 Noto Serif SC 大字号)|

把预览存到 `.claude-design/slide-previews/`(`style-a.html / style-b.html / style-c.html`)。每个预览自包含、约 50–100 行、展示一张带入场动画的标题页。**预览本身已经要按用户的主要语言写**(中文就用中文实际文案,不要 lorem ipsum,不要"标题占位"),让用户判断字体在中文里到底好不好看。

自动在浏览器里打开三个预览。

### 阶段 2.3:用户挑选

问(header: `"风格"`):你更喜欢哪一个?选项:风格 A:[名称] / 风格 B:[名称] / 风格 C:[名称] / 想混搭

如果选混搭,问具体想要哪一边的什么。

---

## 阶段 3:生成完整演示

用阶段 1 的内容(文字 / 文字+图片) + 阶段 2 的风格生成完整 HTML。

如果有图,大纲已经在阶段 1.2 把图融进去了。如果没图,**用 CSS 生成的视觉**(渐变、几何、纹理、印章)做填充——这是一等支持的路径,不是降级方案。

**生成前必读这三份(不读就生成,就一定会破规则)**:

- [html-template.md](html-template.md) — HTML 骨架与 JS 功能集
- [viewport-base.css](viewport-base.css) — 强制内联的视口 CSS
- [animation-patterns.md](animation-patterns.md) — 动画速查与**中文排版规范**

**关键要求**:

- 单文件自包含,CSS/JS 全内联
- 把 `viewport-base.css` **完整**内联进 `<style>` 块
- 中文场景的 `<html lang="zh-CN">`,英文场景 `lang="en"`,混排按主体语言设
- 字体堆栈遵循:`英文显示字体, 中文显示字体, system-ui, sans-serif` 的回退顺序,让数字/拉丁字符走漂亮的英文字体,汉字走选定的中文字体
- 字体加载:Fontshare / Google Fonts(英文) + Google Fonts CJK / jsdelivr CDN(中文,如 LXGW WenKai、得意黑 Smiley Sans)
- 每个区块加清晰注释 `/* === SECTION NAME === */`,且**注释也用中文写**(让用户后续 hand-edit 时看得懂)
- 启用中文排版增强:`text-spacing: ideograph-alpha ideograph-numeric ideograph-special;`、`hanging-punctuation: first allow-end;`、`line-break: strict;`(后两者在引用块、长段落里尤其重要)

---

## 阶段 4:PPT 转换

转换 PowerPoint 流程:

1. **抽取内容** — 跑 `python scripts/extract-pptx.py <input.pptx> <output_dir>`(若未装 python-pptx:`pip install python-pptx`)
2. **跟用户确认** — 展示提取的标题、内容摘要、图片数量。**对中文 PPT 特别注意检查**:文本里有没有乱码、有没有用图片代替文字(扫描件常见)、是否包含繁体而需要转简体
3. **审美选择** — 进入阶段 2
4. **生成 HTML** — 保留全部文字、图片(放在 `assets/`)、顺序、备注(作为 HTML 注释)。**中文 PPT 转出来如果原版用了不合适的微软雅黑/宋体,默认替换为本仓库的中文字体堆栈**(在转换提示里告诉用户做了这一步,允许他们要求保留原字体)

---

## 阶段 5:交付

1. **清理** — 删 `.claude-design/slide-previews/`(若存在)
2. **打开** — `open [filename].html` 在浏览器里看
3. **总结**(用中文跟用户说):
   - 文件位置、风格名、页数
   - 导航方式:方向键、空格、滚轮/滑动、点导航点
   - 改样式去哪儿:`:root` 里改颜色变量;`<link>` 里改字体;`.reveal` 改动画
   - 如果开了浏览器内编辑:鼠标移到左上角或按 E 进入编辑模式,点任意文字改,Ctrl+S 保存

---

## 阶段 6:分享 & 导出(可选)

交付完成后,**主动问**:_"要分享吗?可以部署成在线链接(手机平板也能开),也可以导出 PDF。"_

选项:
- **部署到链接** — 可分享 URL,任意设备
- **导出 PDF** — 万能格式,邮件 / 文档 / 打印
- **两个都要**
- **不需要**

用户拒绝就停在阶段 5。

### 6A:部署到链接(Vercel)

部署到 Vercel(免费)。链接在任何设备打得开,直到用户删除项目为止。

**如果用户第一次用,逐步引导**:

1. **检查 Vercel CLI** — 跑 `npx vercel --version`。没装就先装 Node.js(macOS:`brew install node`)
2. **检查登录** — `npx vercel whoami`
   - 没登录:解释 _"Vercel 是免费的托管服务,要个账号。我带你走一遍:"_
     - 第 1 步:浏览器打开 https://vercel.com/signup
     - 第 2 步:用 GitHub / Google / 邮箱注册,哪个方便用哪个
     - 第 3 步:`vercel login`,按提示在浏览器里授权
     - 第 4 步:`vercel whoami` 确认登录
   - **等用户确认登好之后再继续**
3. **部署** — `bash scripts/deploy.sh <演示路径>`。脚本接受目录(含 index.html)或单个 HTML 文件
4. **把 URL 给用户** — 告诉他/她:
   - 在线 URL(从脚本输出里取)
   - 任意设备能开,可微信/Slack/邮件直接发
   - 想下线:去 https://vercel.com/dashboard 删项目
   - 免费额度足够个人用

**⚠ 部署的坑**:
- **本地图片/视频必须随 HTML 一起上传**。脚本会自动检测 `src="..."` 引用并打包,但 CSS 里 `background-image` 引用的或路径奇怪的可能漏掉。**部署后**打开线上 URL 检查图是否都加载;有缺,最稳的修法是把 HTML 和所有资源放进一个文件夹一起部署
- **多资源演示直接部署文件夹更稳**:`bash scripts/deploy.sh ./my-deck/`
- **文件名带空格能跑但容易出问题**:Vercel URL 把空格编码为 `%20`。如果可以,把空格改成连字符
- **重新部署 = 覆盖同一 URL**。不需要重新分享新链接

### 6B:导出 PDF

每张幻灯片截图后合成 PDF,适合邮件附件、文档嵌入、打印。

**告诉用户**:动画和交互不保留 — 是静态快照,这是正常的。

1. **跑导出脚本**:`bash scripts/export-pdf.sh <html路径> [输出.pdf]`。不指定输出就保存到 HTML 同目录
2. **背后做的事**(简单说一下):无头浏览器以 1920×1080 打开 → 逐页截图 → 合成 PDF → 需要 Playwright(自动装)
3. **Playwright 装不上**:常见是 Chromium 下不下来:`npx playwright install chromium`。再不行就是网络问题
4. **交付**:脚本会自动打开。告诉用户:文件位置、大小、能用在哪里、动画变成最终状态(还是好看的)

**⚠ PDF 导出的坑**:
- **首次跑很慢** — 要装 Playwright 和下载 Chromium(~150MB),提醒用户首次 30–60 秒,后续会快
- **幻灯片必须用 `class="slide"`** — 脚本靠这个选择器找页。本仓库生成的都是,只是手写或别人给的 HTML 可能不是,要先改
- **本地图必须能通过 HTTP 加载** — 脚本起本地服务再访问,所以必须用相对路径(`src="photo.png"`)不能用绝对路径(`src="/Users/.../photo.png"`)
- **大演示 → 大 PDF**:18 页约 20MB。超过 10MB 就主动问 _"要不要压缩?清晰度略降但小一半。"_,用 `--compact` 标志(1280×720 替代 1920×1080)

---

## 配套文件

| 文件                                             | 用途                                                                                         | 何时读                      |
| ------------------------------------------------ | -------------------------------------------------------------------------------------------- | --------------------------- |
| [STYLE_PRESETS.md](STYLE_PRESETS.md)             | 12 套精选预设(每套配中英双字体堆栈) + 3 套中文原生预设                                     | 阶段 2(选风格)            |
| [viewport-base.css](viewport-base.css)           | 强制内联的视口 CSS(已含中文阅读校准)                                                       | 阶段 3(生成)              |
| [html-template.md](html-template.md)             | HTML 骨架、JS 功能集、中文骨架与字体堆栈                                                      | 阶段 3(生成)              |
| [animation-patterns.md](animation-patterns.md)   | CSS/JS 动画速查 + **中文排版规范**(全角标点、避头尾、首行缩进、悬挂标点等)                  | 阶段 3(生成)              |
| [scripts/extract-pptx.py](scripts/extract-pptx.py) | Python PPT 抽取脚本                                                                          | 阶段 4(转换)              |
| [scripts/deploy.sh](scripts/deploy.sh)           | 部署到 Vercel                                                                                | 阶段 6(分享)              |
| [scripts/export-pdf.sh](scripts/export-pdf.sh)   | 导出 PDF                                                                                     | 阶段 6(分享)              |
