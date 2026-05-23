# Slides CN(简体中文原生版)

> **English summary**: `slides-cn` is a Claude Code skill that creates stunning, animation-rich HTML presentations — from scratch or by converting PowerPoint files. **This is a Simplified-Chinese-native fork of [`frontend-slides`](https://github.com/zarazhangrui/frontend-slides)**: renamed to `slides-cn` to avoid collision with the upstream plugin. Every preset has a curated Chinese font pairing, CSS rules cover full-width punctuation / line-break / hanging-punctuation / `palt` / 2-em first-line indent / per-language font stacks, and content-density limits are recalibrated for CJK character density. English content is still fully supported.

> **本分叉已重命名为 `slides-cn`**,以避免与上游 `frontend-slides` 插件在 marketplace、skill name、`/slash` 命令上撞车。两个插件可以并存安装。

为非设计师做出**单文件、零依赖、动画驱动的 HTML 演示文稿**——可以从零开始,也可以把 PowerPoint 转成网页。

**本分叉是简体中文原生版**:12 套预设全部补齐中文字体配对,新增 3 套中文专属预设(宣纸水墨 / 港式霓虹 / 杂志感 CN),CSS 规则按汉字方块结构重新校准了行高、字号下限、避头尾、悬挂标点、首行缩进、`palt` 字距等。**英文内容、中英混排同样是一等支持目标。**

## 这份技能能做什么

- **零依赖** —— 单个 HTML 文件,CSS / JS 全部内联。不用 npm、不打包、不上框架。
- **看图说话(Show, Don't Tell)** —— 说不清自己想要什么样的设计?没关系,直接生成 3 张视觉预览让你挑。
- **PPT 转换** —— 把现有的 PowerPoint 转成网页演示,保留全部图文和备注。
- **拒绝 AI 流水线感** —— 精选 15 套有性格的视觉风格(不要白底紫渐变 + Inter)。
- **生产级质量** —— 可访问、响应式、注释清晰的代码,你拿走能直接改。

## 中文专属增强

- **字体堆栈双层化**:每个预设给出"英文字体 + 中文字体"的完整 `font-family`,数字 / 拉丁自动走漂亮的英文字体,汉字走选定的中文字体。
- **15 套预设**:原 12 套预设全部补中文配对,新增 3 套中文原生预设:
  - **宣纸水墨**(Ma Shan Zheng + LXGW WenKai + 朱砂印章)—— 东方、传统、考究
  - **港式霓虹**(Noto Serif HK + 霓虹勾边)—— 街招、夜色、复古港风
  - **杂志感 CN**(Noto Serif SC Black + 横线分隔 + 引言块)—— 沉稳可信的中文 editorial 替代
- **`viewport-base.css` 中文规则**:`line-break: strict`(避头尾)、`hanging-punctuation`(悬挂标点)、`text-spacing` + `palt`(字距优化)、`line-height: 1.7`、字号下限提升到 14px(避免汉字小于 12px 退化)、`overflow-wrap: anywhere` 长 URL 兜底,全部默认开启。
- **内容密度上限按汉字校准**:标题 ≤ 12 字 / 副标 ≤ 20 字 / 要点 ≤ 15 字 / 段落 ≤ 60 字 / 卡片标题 ≤ 6 字。
- **中文字体 CDN 加载速查**:Noto Sans/Serif SC、LXGW WenKai、Smiley Sans 得意黑、ZCOOL 系列、HarmonyOS Sans SC、Sarasa Mono SC、Maoken Tangyuan 等开源中文 web 字体,加载方式全部写在 `STYLE_PRESETS.md`。
- **中英混排支持**:`<html lang="zh-CN">` 自动激活全套中文规则;遇到英文/数字片段自动走拉丁字体回退;`font-feature-settings: "palt"` 解决中英之间字距过宽。

## 安装

### 通过 Plugin Marketplace(推荐)

在 Claude Code 里两行命令搞定:

```bash
/plugin marketplace add Afee2019/frontend-slides
/plugin install slides-cn@slides-cn
```

(GitHub 仓库本身还叫 `frontend-slides`,只是插件 / skill 标识改成了 `slides-cn`。)

然后输 `/slides-cn` 即可调用。

### 手工安装

复制本仓库到 Claude Code 的 skills 目录:

```bash
git clone https://github.com/Afee2019/frontend-slides.git ~/.claude/skills/slides-cn
```

或者只拷文件:

```bash
mkdir -p ~/.claude/skills/slides-cn/scripts
cp SKILL.md STYLE_PRESETS.md viewport-base.css html-template.md animation-patterns.md ~/.claude/skills/slides-cn/
cp scripts/extract-pptx.py scripts/deploy.sh scripts/export-pdf.sh ~/.claude/skills/slides-cn/scripts/
```

输 `/slides-cn` 调用。

## 用法

### 从零做一份演示

```
/slides-cn

> "帮我做一份 AI 创业公司的融资路演 PPT"
```

技能会:

1. 一次性问完用途 / 篇幅 / 内容就绪度 / 主要语言 / 是否需要浏览器内编辑
2. 问你想让观众有什么感受(沉稳 / 振奋 / 平和 / 共鸣 / 东方书卷)
3. 生成 3 张视觉预览让你对比
4. 在你选的风格里生成完整演示
5. 在浏览器里打开

### PPT 转换

```
/slides-cn

> "把 presentation.pptx 转成网页演示"
```

技能会:

1. 提取全部文字、图片、备注
2. 给你确认提取结果
3. 让你选风格
4. 生成完整 HTML(保留原图,文字 / 备注作为注释)

## 包含的风格

### 暗色主题

- **Bold Signal** —— 自信、高冲击、暗底彩卡(中文配 Smiley Sans 得意黑)
- **Electric Studio** —— 干净、专业、双栏切分(中文配 HarmonyOS Sans SC)
- **Creative Voltage** —— 活力、复古现代、电光蓝 + 霓虹(中文配 ZCOOL 庆科黄油体)
- **Dark Botanical** —— 雅致、考究、暖色点缀(中文配思源宋体 + LXGW 文楷)

### 浅色主题

- **Notebook Tabs** —— 编辑、有秩序、彩色标签(中文配思源宋体 Black + 思源黑体)
- **Pastel Geometry** —— 友好、可接近、垂直药丸(中文配猫啃汤圆体)
- **Split Pastel** —— 俏皮、现代、双色分屏(中文配 ZCOOL 快乐体)
- **Vintage Editorial** —— 有人格、复古衬线 + 几何(中文配 ZCOOL 小薇 + LXGW Bright)

### 专题

- **Neon Cyber** —— 未来感、霓虹辉光(中文配 Smiley Sans)
- **Terminal Green** —— 开发者、黑客感(中文配更纱等宽 Sarasa Mono SC)
- **Swiss Modern** —— 包豪斯、极简(中文配思源黑体单字体)
- **Paper & Ink** —— 文学、思辨、首字下沉(中文配思源宋体 + LXGW 文楷)

### 中文原生(新增)

- **宣纸水墨** —— 东方、传统、朱砂印章 + 楷书
- **港式霓虹** —— 港风、霓虹招牌、深紫底
- **杂志感 CN** —— 沉稳、可信、巨字宋体 + 横线分隔

## 架构

本技能用**渐进披露**——主 `SKILL.md` 只是一份精炼地图(约 200 行),其余文件按阶段按需读取:

| 文件                      | 用途                                          | 何时加载                |
| ------------------------- | --------------------------------------------- | ----------------------- |
| `SKILL.md`                | 主流程 + 规则                                  | 永远(调用即载)        |
| `STYLE_PRESETS.md`        | 15 套预设 + 中文字体配对                       | 阶段 2(选风格)        |
| `viewport-base.css`       | 强制内联的 CSS(含中文阅读校准)               | 阶段 3(生成)          |
| `html-template.md`        | HTML 骨架 + JS 功能 + 中文自检清单             | 阶段 3(生成)          |
| `animation-patterns.md`   | 动画 + **中文排版规范**                        | 阶段 3(生成)          |
| `scripts/extract-pptx.py` | PPT 内容提取                                   | 阶段 4(转换)          |
| `scripts/deploy.sh`       | 部署到 Vercel                                  | 阶段 6(分享)          |
| `scripts/export-pdf.sh`   | 导出 PDF                                       | 阶段 6(分享)          |

这种设计借鉴 [OpenAI 的 harness engineering](https://openai.com/index/harness-engineering/):"给 agent 一份地图,不要丢给它一本 1000 页的说明书。"

## 设计理念

- **不会设计,也能做出好东西。**你不需要会画 UI,你只需要看到觉得"对"。
- **依赖即债务。**一个 HTML 文件 10 年后还能开。2019 年的 React 项目?祝你好运。
- **通用即遗忘。**每一份演示应该感觉是为这次内容专门定做的,不是模板套出来的。
- **注释是一种善意。**代码要能讲清自己——给未来的你,也给任何打开它的人。

## 分享演示

生成之后,技能会主动问你要不要分享。两条路:

### 部署到在线 URL

一行命令把演示发到一个永久、可分享的链接(手机平板都能开):

```bash
bash scripts/deploy.sh ./my-deck/
# 或
bash scripts/deploy.sh ./presentation.html
```

用 [Vercel](https://vercel.com) 免费托管。第一次会引导你注册登录。

### 导出 PDF

把演示转成 PDF,用于邮件、Slack、Notion、打印:

```bash
bash scripts/export-pdf.sh ./my-deck/index.html
bash scripts/export-pdf.sh ./presentation.html ./output.pdf
```

用 [Playwright](https://playwright.dev) 把每页截图为 1920×1080 后合成 PDF。需要时自动装。**动画不保留**——是静态快照。

## 依赖要求

- [Claude Code](https://claude.ai/claude-code) CLI
- PPT 转换:Python + `python-pptx`
- URL 部署:Node.js + Vercel 账号(免费)
- PDF 导出:Node.js(Playwright 自动装)

## 致谢

原版由 [@zarazhangrui](https://github.com/zarazhangrui) 用 Claude Code 创建。

中文原生分叉:补齐 12 套预设的中文字体配对、新增 3 套中文专属预设、改写 `animation-patterns.md` 加"中文排版规范"、`viewport-base.css` 按汉字密度校准、`SKILL.md` 阶段提问全部中文化、`html-template.md` 增加中文骨架与自检清单。

## License

MIT —— 拿去用、改、分享。
