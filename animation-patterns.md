# 动画与排版速查(中文原生版)

生成演示时按此速查匹配动画与情绪;**中文场景额外读"中文排版规范"那一节,它是默认必要操作,不是可选**。

## 情绪 → 效果对照

| 情绪              | 动画                                                       | 视觉线索                                                          |
| ----------------- | ---------------------------------------------------------- | ----------------------------------------------------------------- |
| **戏剧 / 电影感** | 慢淡入(1–1.5s)、大尺度 scale 过渡(0.9 → 1)、视差滚动 | 暗背景、聚光、满版图像                                            |
| **科技 / 未来感** | 霓虹辉光(box-shadow)、文字 glitch / scramble、网格揭示  | 粒子系统、网格底纹、等宽点缀、青/品红/电光蓝                      |
| **俏皮 / 友好**   | 弹簧缓动(spring physics)、漂浮 / 浮动                    | 圆角、亮色 / 粉彩、手绘元素                                       |
| **专业 / 企业**   | 200–300ms 的克制动画、干净切换                             | 海军蓝 / 石板灰 / 炭黑、精确间距、数据可视化                      |
| **平和 / 极简**   | 极慢、微弱位移、温和淡入                                   | 大留白、低饱和、衬线字、宽内边距                                  |
| **编辑 / 杂志**   | 文字错峰揭示、图文穿插                                     | 强字体层级、引言块、破网格版式、衬线大标 + 无衬线正文              |
| **东方 / 书卷**   | 墨晕扩散、印章按下时的微缩 + 旋转、首字下沉                | 宣纸纹理、朱砂印、笔锋楷书、留白(留白也是构图!),纯黑灰朱三色  |

## 入场动画

```css
/* 淡入 + 向上(最通用) */
.reveal {
    opacity: 0;
    transform: translateY(30px);
    transition: opacity 0.6s var(--ease-out-expo),
                transform 0.6s var(--ease-out-expo);
}
.visible .reveal {
    opacity: 1;
    transform: translateY(0);
}

/* 缩放进入 */
.reveal-scale {
    opacity: 0;
    transform: scale(0.9);
    transition: opacity 0.6s, transform 0.6s var(--ease-out-expo);
}

/* 从左滑入 */
.reveal-left {
    opacity: 0;
    transform: translateX(-50px);
    transition: opacity 0.6s, transform 0.6s var(--ease-out-expo);
}

/* 模糊淡入 */
.reveal-blur {
    opacity: 0;
    filter: blur(10px);
    transition: opacity 0.8s, filter 0.8s var(--ease-out-expo);
}

/* 墨晕扩散(中式专用) */
.reveal-ink {
    opacity: 0;
    filter: blur(16px);
    transform: scale(1.05);
    transition: opacity 1.2s ease-out,
                filter 1.2s ease-out,
                transform 1.2s ease-out;
}
.visible .reveal-ink {
    opacity: 1;
    filter: blur(0);
    transform: scale(1);
}

/* 印章按下(中式专用) */
@keyframes seal-press {
    0%   { opacity: 0; transform: scale(1.3) rotate(8deg); filter: blur(2px); }
    60%  { opacity: 1; transform: scale(0.95) rotate(-1deg); filter: blur(0); }
    80%  { transform: scale(1.02) rotate(-3deg); }
    100% { transform: scale(1) rotate(-3deg); }
}
.seal {
    animation: seal-press 0.8s var(--ease-out-expo) both;
    animation-delay: 1.2s; /* 在标题出现之后再"盖章" */
}
```

## 背景效果

```css
/* 渐变网格 —— 叠加多层径向渐变制造空气感 */
.gradient-bg {
    background:
        radial-gradient(ellipse at 20% 80%, rgba(120, 0, 255, 0.3) 0%, transparent 50%),
        radial-gradient(ellipse at 80% 20%, rgba(0, 255, 200, 0.2) 0%, transparent 50%),
        var(--bg-primary);
}

/* 噪点纹理 —— 内联 SVG 颗粒 */
.noise-bg {
    background-image: url("data:image/svg+xml,..."); /* 内联 SVG 噪点 */
}

/* 网格底纹 —— 微弱结构线 */
.grid-bg {
    background-image:
        linear-gradient(rgba(255,255,255,0.03) 1px, transparent 1px),
        linear-gradient(90deg, rgba(255,255,255,0.03) 1px, transparent 1px);
    background-size: 50px 50px;
}

/* 宣纸纹理(中式专用)—— 用 SVG turbulence + 多层偏移 */
.xuan-paper-bg {
    background-color: #f3ead3;
    background-image:
        radial-gradient(circle at 30% 20%, rgba(184, 146, 77, 0.08) 0%, transparent 50%),
        radial-gradient(circle at 70% 70%, rgba(192, 57, 43, 0.05) 0%, transparent 50%),
        url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='200' height='200'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.85' numOctaves='2'/%3E%3CfeColorMatrix values='0 0 0 0 0.55 0 0 0 0 0.45 0 0 0 0 0.30 0 0 0 0.18 0'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23n)'/%3E%3C/svg%3E");
}
```

## 交互效果

```javascript
/* 悬停 3D 倾斜 —— 给卡片 / 面板增加纵深 */
class TiltEffect {
    constructor(element) {
        this.element = element;
        this.element.style.transformStyle = 'preserve-3d';
        this.element.style.perspective = '1000px';

        this.element.addEventListener('mousemove', (e) => {
            const rect = this.element.getBoundingClientRect();
            const x = (e.clientX - rect.left) / rect.width - 0.5;
            const y = (e.clientY - rect.top) / rect.height - 0.5;
            this.element.style.transform = `rotateY(${x * 10}deg) rotateX(${-y * 10}deg)`;
        });

        this.element.addEventListener('mouseleave', () => {
            this.element.style.transform = 'rotateY(0) rotateX(0)';
        });
    }
}
```

## 故障速查

| 问题            | 修法                                                                              |
| --------------- | --------------------------------------------------------------------------------- |
| 字体没加载      | 检查 Fontshare / Google Fonts URL,确认 CSS 里 font-family 字符串完全匹配         |
| 动画没触发      | 检查 Intersection Observer 在跑,确认 `.visible` 真的被加上去                     |
| 滚动吸附失效    | `scroll-snap-type: y mandatory;` 要在 `html` 上;每张 `.slide` 都要 `scroll-snap-align: start;` |
| 移动端崩        | 768px 断点关掉重特效;测触摸事件;粒子数减半                                     |
| 性能差          | 谨慎用 `will-change`;只用 `transform`/`opacity` 类动画;滚动 handler 上 throttle |
| 中文换行难看    | 见下方"中文排版规范",启用 `line-break: strict;` + `word-break: normal;`         |
| 中英混排字距太宽 | `font-feature-settings: "palt";` 启用比例字宽                                    |
| 中文字号一缩就糊 | `clamp()` 的下限 ≥ `0.875rem`(~14px);汉字 < 12px 在主流屏上严重退化             |

---

# 中文排版规范

中文场景下,以下规则是**默认必要操作**,不是可选润色。生成 HTML 时,在 `viewport-base.css` 之上**追加**一个 `[lang^="zh"]` 作用域的样式块,把这些规则全部启用。

## 1. 全角标点 vs 半角标点

中文内容里使用全角标点(`，。；:!?「」『』、（）`),不要用半角(`,.;:!?\"\"''()`)。这不是审美选择,是**汉字之间的视觉节奏**问题——半角标点会让段落看起来"漏气"。

生成的内容文本里,默认产出全角;只有在 URL、代码、数字范围(`5-10`)等场景保留半角。

## 2. 标点挤压(`text-spacing-trim`)与 `palt`

CJK 标点默认带"全角空间"——句号占满一个字身宽度,但右边的留白会让段落看起来稀疏。两种解法:

```css
/* 现代浏览器(Chromium 117+ / Safari 18+):由 CSS 标点挤压控制 */
[lang^="zh"] {
    text-spacing-trim: trim-start;            /* 行首标点不留白 */
    text-spacing: ideograph-alpha ideograph-numeric ideograph-special;
}

/* 兼容老浏览器:OpenType `palt` 让中文标点按比例宽度排 */
[lang^="zh"] body,
.cjk {
    font-feature-settings: "palt" 1, "kern" 1;
}
```

`palt` 一旦开启,所有句号、逗号、引号都会被收缩到合适宽度,中英混排时不会出现"中文逗号后突然一大块空气再接英文"的尴尬。

## 3. 避头尾(`line-break`)

中文不允许行首是 `。、,!?:;)」』】` 等收口标点,也不允许行尾是 `「『(【` 等开口标点——这叫"避头尾"。CSS 一行启用:

```css
[lang^="zh"] {
    line-break: strict;                  /* 严格避头尾 */
    word-break: normal;                  /* 不允许在汉字间硬断 */
    overflow-wrap: anywhere;             /* 长 URL / 代码兜底 */
    hyphens: none;                       /* 中文不连字符 */
}
```

`word-break: break-all` 是反模式——它会让长段落出现"半个词被劈成两半"的难看断行。**只在表格、卡片标题这种宽度极受限的容器里**才考虑 `word-break: break-word`(英文)/`break-all`(中英混排兜底)。

## 4. 首行缩进 2em

中文正文段落传统上首行缩进 2 个字。技术演示页不一定要,**长段落叙述、报道、文学类内容必加**:

```css
[lang^="zh"] .prose p,
[lang^="zh"] .article p {
    text-indent: 2em;
}

/* 引言、列表项不缩进 */
[lang^="zh"] .quote,
[lang^="zh"] li {
    text-indent: 0;
}
```

短句、单行强调文案不缩进。**判断标准**:这段是"被读的"还是"被看的"。被读的(段落)缩,被看的(标语)不缩。

## 5. 悬挂标点(`hanging-punctuation`)

`hanging-punctuation: first allow-end;` 让行首标点(尤其引号)悬挂到版心之外,版心边缘更整齐:

```css
body {
    hanging-punctuation: first allow-end last;
}
```

目前 Safari 完整支持,Chromium 部分支持。失败会优雅退化,**没有副作用,放心写**。

## 6. 行高 1.6–1.8

汉字方块结构,行高低于 1.5 会让段落看起来挤;高于 1.9 又会"散架"。默认 1.7,大字号标题可降到 1.2:

```css
[lang^="zh"] body { line-height: 1.7; }
[lang^="zh"] h1, [lang^="zh"] .title-display { line-height: 1.15; }
[lang^="zh"] h2, [lang^="zh"] h3 { line-height: 1.3; }
[lang^="zh"] .quote { line-height: 1.8; }   /* 引文更舒展 */
```

## 7. 字号下限

```css
[lang^="zh"] :root {
    /* 汉字 < 12px 在 1× DPR 屏上严重退化,clamp 下限提到 14px 起 */
    --body-size: clamp(0.875rem, 1.5vw, 1.125rem);
    --small-size: clamp(0.75rem, 1.1vw, 0.95rem);  /* 注脚最小 12px */
}
```

英文最小可读 9–10px;中文最小 12–13px。**生成时,带汉字的任意元素字号不得低于 12px**。注脚、版权、徽章上的小字也要遵守。

## 8. 中英混排空格

中英文之间应当有一个"四分之一空格",但 HTML 里多打一个空格又会被压缩。三种现实做法,优先级递减:

```html
<!-- 法 1:CSS text-spacing 现代标准(推荐) -->
<p style="text-spacing: ideograph-alpha ideograph-numeric;">这是一份 React 演示</p>

<!-- 法 2:手动加空格(最稳,但要在写文案时注意) -->
<p>这是一份 React 演示</p>

<!-- 法 3:JS 自动加空格(适合 PPT 转换后批量处理) -->
<script>
// 在中英之间插入半角空格
function pangu(text) {
    return text
        .replace(/([一-龥])([A-Za-z0-9])/g, '$1 $2')
        .replace(/([A-Za-z0-9])([一-龥])/g, '$1 $2');
}
</script>
```

**默认走法 1 + 法 2**:生成文案时手动加空格,CSS 上再叠 `text-spacing`。法 3 只在阶段 4 PPT 转换里用,因为原 PPT 文案不归我们写。

## 9. 数字 / 拉丁字符走英文字体

字体堆栈写对了,数字和拉丁字符自动落到英文字体上。**前提是英文字体必须放在 `font-family` 列表的最前面**:

```css
/* 对 —— 拉丁先匹配,匹配不到再落到中文字体 */
font-family: "Archivo Black", "Smiley Sans", "Noto Sans SC", system-ui, sans-serif;

/* 错 —— 中文字体在前会接管所有字符,包括数字,数字会变成"中文字体里的数字",通常很丑 */
font-family: "Noto Sans SC", "Archivo Black", system-ui, sans-serif;
```

例外是 `JetBrains Mono` + `Sarasa Mono SC` 这种**特意要等宽对齐**的场景——两者字宽精心设计过,顺序对调不会出问题。

## 10. 数字字符替代字形(`tnum` / `lnum`)

数字密集的场景(表格、计数器、数据可视化)启用等宽数字 `tnum` 和衬线字体的 lining figures `lnum`:

```css
.data, .number-grid, .counter {
    font-feature-settings: "tnum" 1, "lnum" 1;
    font-variant-numeric: tabular-nums lining-nums;
}
```

## 11. 标点和引号不要"乱跨语言"

```
✓ 中文段落用 "" 「」                       <!-- 全角或直角引号 -->
✓ 英文段落用 ""                            <!-- 弯引号 -->
✗ 中文段落里出现 "Hello"(英文直引号)      <!-- 视觉断裂 -->
```

引号正确选型不只是审美——它直接影响段落的"视觉一致性"。如果用户的 PPT 抽出来是混着的,在阶段 4 的清洗步骤里**统一一次**。

## 12. 大字号标题的特殊处理

```css
[lang^="zh"] .title-display {
    font-size: clamp(2.5rem, 9vw, 7rem);
    font-weight: 900;
    line-height: 0.95;                  /* 大字号要压低行高,字与字几乎咬合 */
    letter-spacing: -0.02em;             /* 中文字间距通常 0,大字时收一点更紧凑 */
    font-feature-settings: "palt" 1;
}
```

中文标题字间距(`letter-spacing`)通常保持 0,但**字号超过 4rem** 后,字之间的"空气"会显得稀薄,需要 `-0.01em ~ -0.03em` 收一点。

---

## 一份合规的 `[lang^="zh"]` 样式块模板

```css
[lang^="zh"] {
    line-break: strict;
    word-break: normal;
    overflow-wrap: anywhere;
    hyphens: none;
    text-spacing: ideograph-alpha ideograph-numeric ideograph-special;
    text-spacing-trim: trim-start;
    font-feature-settings: "palt" 1, "kern" 1;
    hanging-punctuation: first allow-end last;
}
[lang^="zh"] body { line-height: 1.7; }
[lang^="zh"] h1, [lang^="zh"] .title-display { line-height: 1.15; letter-spacing: -0.02em; }
[lang^="zh"] h2, [lang^="zh"] h3 { line-height: 1.3; }
[lang^="zh"] .prose p { text-indent: 2em; }
[lang^="zh"] .quote { line-height: 1.8; text-indent: 0; }

[lang^="zh"] :root {
    --body-size: clamp(0.875rem, 1.5vw, 1.125rem);
    --small-size: clamp(0.75rem, 1.1vw, 0.95rem);
    --cjk-line-height: 1.7;
}
```

**把这段放在 `viewport-base.css` 之后内联**,它假定 `viewport-base.css` 已经先生效。如果你写的页面 `<html lang="zh-CN">`,上面这段就会自动生效;`lang="en"` 的页面不会被影响。
