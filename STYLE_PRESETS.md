# 风格预设速查(中文原生版)

Frontend Slides 的精选视觉风格预设。每一套都来自真实设计参考——不要"AI 流水线感"。**仅使用抽象图形,不画插图。**

每个预设给出**双字体堆栈**:英文字体保持原版选型不动,中文部分单独配对。生成时按主要语言决定哪一头放在 `font-family` 首位,另一头作为回退,让数字 / 拉丁字符自动走漂亮的英文字体、汉字走选定的中文字体。

**视口 CSS**:必须包含的基线样式见 [viewport-base.css](viewport-base.css)。**中文排版规范**见 [animation-patterns.md](animation-patterns.md#中文排版规范)。

## 中文字体加载速查

| 字体名称(`font-family` 用) | 来源       | 加载方式                                                                                                  | 性格                       |
| ----------------------------- | ---------- | --------------------------------------------------------------------------------------------------------- | -------------------------- |
| `"Noto Sans SC"`              | Google     | `https://fonts.googleapis.com/css2?family=Noto+Sans+SC:wght@300;400;500;700;900&display=swap`              | 现代黑体,中性百搭         |
| `"Noto Serif SC"`             | Google     | `https://fonts.googleapis.com/css2?family=Noto+Serif+SC:wght@300;400;600;700;900&display=swap`             | 现代宋体,多 weight        |
| `"Noto Serif HK"`             | Google     | `https://fonts.googleapis.com/css2?family=Noto+Serif+HK:wght@500;700;900&display=swap`                     | 港式宋体,招牌感           |
| `"LXGW WenKai"` 霞鹜文楷       | jsdelivr   | `https://cdn.jsdelivr.net/npm/lxgw-wenkai-webfont@1.7.0/style.css`                                         | 楷书柔笔,书卷气           |
| `"LXGW Bright"`               | jsdelivr   | `https://cdn.jsdelivr.net/npm/lxgw-bright-webfont@latest/style.css`                                        | 楷骨黑肉,现代书卷         |
| `"Smiley Sans"` 得意黑         | jsdelivr   | `https://cdn.jsdelivr.net/gh/atelier-anchor/smiley-sans@1.1.1/dist/index.css`(用 `Smiley Sans Oblique`) | 粗壮倾斜,现代展示         |
| `"ZCOOL QingKe HuangYou"`     | Google     | `https://fonts.googleapis.com/css2?family=ZCOOL+QingKe+HuangYou&display=swap`                              | 粗壮复古,招牌字           |
| `"ZCOOL KuaiLe"`              | Google     | `https://fonts.googleapis.com/css2?family=ZCOOL+KuaiLe&display=swap`                                       | 圆润友好,儿童感           |
| `"ZCOOL XiaoWei"`             | Google     | `https://fonts.googleapis.com/css2?family=ZCOOL+XiaoWei&display=swap`                                      | 细瘦展示,雅致             |
| `"Ma Shan Zheng"` 马善政楷书   | Google     | `https://fonts.googleapis.com/css2?family=Ma+Shan+Zheng&display=swap`                                      | 楷书笔触,题字感           |
| `"Long Cang"` 龙藏体           | Google     | `https://fonts.googleapis.com/css2?family=Long+Cang&display=swap`                                          | 行书飘逸                   |
| `"Liu Jian Mao Cao"` 柳建毛草  | Google     | `https://fonts.googleapis.com/css2?family=Liu+Jian+Mao+Cao&display=swap`                                   | 草书急笔                   |
| `"Sarasa Mono SC"` 更纱等宽    | jsdelivr   | `https://cdn.jsdelivr.net/npm/cn-fontsource-sarasa-mono-sc-regular/font.min.css`                            | 中文等宽,代码场景         |
| `"HarmonyOS Sans SC"` 鸿蒙黑   | jsdelivr   | `https://cdn.jsdelivr.net/npm/cn-fontsource-harmony-os-sans-sc-regular/font.min.css`                       | 几何现代,科技感           |
| `"MiSans"` 米兰亭              | jsdelivr   | `https://cdn.jsdelivr.net/npm/cn-fontsource-mi-sans-medium/font.min.css`                                   | 屏显优化,中性             |
| `"Maoken Tangyuan"` 猫啃汤圆   | jsdelivr   | `https://cdn.jsdelivr.net/npm/cn-fontsource-maoken-tangyuan-regular/font.min.css`                          | 圆润,糖果感               |

**默认系统回退链**(写在每个 `font-family` 末端):
```
"PingFang SC", "Hiragino Sans GB", "Microsoft YaHei", system-ui, sans-serif
```
serif 场景换:
```
"Songti SC", "STSong", "SimSun", serif
```

---

## 暗色主题

### 1. Bold Signal

**调性**:自信、大胆、现代、强冲击力

**版式**:暗色渐变 + 一张大色块卡片。序号在左上,导航在右上,标题压在卡片左下。

**字体(英文)**:
- 显示:`Archivo Black`(900)
- 正文:`Space Grotesk`(400/500)

**字体(中文)**:
- 显示:`Smiley Sans 得意黑`(Oblique)— 粗壮、倾斜、几何感,与 Archivo Black 的方块结构同源,中文换上来不会塌
- 正文:`Noto Sans SC`(500/700)
- `font-family: "Archivo Black", "Smiley Sans", "Noto Sans SC", "PingFang SC", system-ui, sans-serif;`

**配色**:
```css
:root {
    --bg-primary: #1a1a1a;
    --bg-gradient: linear-gradient(135deg, #1a1a1a 0%, #2d2d2d 50%, #1a1a1a 100%);
    --card-bg: #FF5722;
    --text-primary: #ffffff;
    --text-on-card: #1a1a1a;
}
```

**标志性元素**:
- 大色块卡片(橙、珊瑚、亮黄等鲜艳色)作视觉焦点
- 巨大的章节号(01、02……)
- 顶部面包屑导航,active / inactive 用透明度区分
- 网格定位,所有元素都贴齐基线

---

### 2. Electric Studio

**调性**:大胆、干净、专业、高对比

**版式**:水平分两栏——上白下蓝,品牌符号压在角落。

**字体(英文)**:
- 显示:`Manrope`(800)
- 正文:`Manrope`(400/500)

**字体(中文)**:
- 显示:`HarmonyOS Sans SC`(700)— 几何现代,与 Manrope 同属"圆角无衬"族
- 正文:`HarmonyOS Sans SC`(400)或 `MiSans`(400)
- `font-family: "Manrope", "HarmonyOS Sans SC", "Noto Sans SC", "PingFang SC", system-ui, sans-serif;`

**配色**:
```css
:root {
    --bg-dark: #0a0a0a;
    --bg-white: #ffffff;
    --accent-blue: #4361ee;
    --text-dark: #0a0a0a;
    --text-light: #ffffff;
}
```

**标志性元素**:
- 两栏垂直分割
- 分割线边缘有强调条
- 引言文字作为主体
- 极简、自信的留白

---

### 3. Creative Voltage

**调性**:大胆、创意、能量、复古现代

**版式**:左右分栏——左电光蓝,右深色;手写体作点缀。

**字体(英文)**:
- 显示:`Syne`(700/800)
- 等宽:`Space Mono`(400/700)

**字体(中文)**:
- 显示:`ZCOOL QingKe HuangYou 站酷庆科黄油体` — 粗壮复古招牌字,跟 Syne 那种"瘦高复古运动感"形成互补
- 等宽:`Sarasa Mono SC 更纱等宽`(400/700)
- `font-family: "Syne", "ZCOOL QingKe HuangYou", "Noto Sans SC", system-ui, sans-serif;`

**配色**:
```css
:root {
    --bg-primary: #0066ff;
    --bg-dark: #1a1a2e;
    --accent-neon: #d4ff00;
    --text-light: #ffffff;
}
```

**标志性元素**:
- 电光蓝 + 霓虹黄强对比
- 半调网点(halftone)纹理
- 霓虹勋章 / 标签
- 手写体作创意点缀

---

### 4. Dark Botanical

**调性**:优雅、考究、艺术、高级

**版式**:暗底居中内容,抽象柔光形状压角。

**字体(英文)**:
- 显示:`Cormorant`(400/600)— 优雅衬线
- 正文:`IBM Plex Sans`(300/400)

**字体(中文)**:
- 显示:`Noto Serif SC`(400/600,Light/Regular)— 配 Cormorant 的纤秀骨架
- 正文:`LXGW WenKai 霞鹜文楷`(Regular)— 楷书柔笔补足"温柔考究"的气质,或退而选 `Noto Sans SC` Light(300)
- `font-family: "Cormorant", "Noto Serif SC", "Songti SC", "STSong", serif;`(显示)
- `font-family: "IBM Plex Sans", "LXGW WenKai", "Noto Sans SC", system-ui, sans-serif;`(正文)

**配色**:
```css
:root {
    --bg-primary: #0f0f0f;
    --text-primary: #e8e4df;
    --text-secondary: #9a9590;
    --accent-warm: #d4a574;
    --accent-pink: #e8b4b8;
    --accent-gold: #c9b896;
}
```

**标志性元素**:
- 抽象柔光圆斑(模糊 + 叠加)
- 暖色强调(粉、金、赭石)
- 极细的竖向强调线
- 斜体签名式排版
- **不画插图——只用 CSS 抽象图形**

---

## 浅色主题

### 5. Notebook Tabs

**调性**:编辑、有秩序、雅致、可触

**版式**:深底上漂浮一张奶白纸张卡片,右边缘伸出彩色标签条。

**字体(英文)**:
- 显示:`Bodoni Moda`(400/700)— 经典编辑衬线
- 正文:`DM Sans`(400/500)

**字体(中文)**:
- 显示:`Noto Serif SC`(900,Black)— 极对比衬线,匹配 Bodoni 的粗细落差
- 正文:`Noto Sans SC`(400/500)或 `LXGW Bright`(增加书卷气)
- `font-family: "Bodoni Moda", "Noto Serif SC", "Songti SC", serif;`(显示)
- `font-family: "DM Sans", "Noto Sans SC", "PingFang SC", system-ui, sans-serif;`(正文)

**配色**:
```css
:root {
    --bg-outer: #2d2d2d;
    --bg-page: #f8f6f1;
    --text-primary: #1a1a1a;
    --tab-1: #98d4bb; /* 薄荷 */
    --tab-2: #c7b8ea; /* 薰衣草 */
    --tab-3: #f4b8c5; /* 樱粉 */
    --tab-4: #a8d8ea; /* 天蓝 */
    --tab-5: #ffe6a7; /* 奶黄 */
}
```

**标志性元素**:
- 纸张卡片带微弱投影
- 右边缘的彩色标签竖排文字
- 左侧装订孔装饰
- 标签字号必须 `clamp(0.5rem, 1vh, 0.7rem)` 跟视口走

---

### 6. Pastel Geometry

**调性**:友好、有秩序、现代、易接近

**版式**:粉彩底色 + 白色卡片,右边缘竖向药丸条。

**字体(英文)**:
- 显示:`Plus Jakarta Sans`(700/800)
- 正文:`Plus Jakarta Sans`(400/500)

**字体(中文)**:
- 显示:`Maoken Tangyuan 猫啃汤圆体` 或 `ZCOOL KuaiLe 站酷快乐体` — 圆润友好,匹配 Plus Jakarta 的圆角几何感
- 正文:`HarmonyOS Sans SC`(400)或 `Noto Sans SC`(400/500)
- `font-family: "Plus Jakarta Sans", "Maoken Tangyuan", "Noto Sans SC", system-ui, sans-serif;`

**配色**:
```css
:root {
    --bg-primary: #c8d9e6;
    --card-bg: #faf9f7;
    --pill-pink: #f0b4d4;
    --pill-mint: #a8d4c4;
    --pill-sage: #5a7c6a;
    --pill-lavender: #9b8dc4;
    --pill-violet: #7c6aad;
}
```

**标志性元素**:
- 圆角卡片 + 柔投影
- **右边缘竖向药丸条**,高度参差(短-中-长-中-短)
- 卡片宽度一致,高度不一致
- 右上角图标按钮

---

### 7. Split Pastel

**调性**:俏皮、现代、友好、有创意

**版式**:左右两色背景对半切(左蜜桃,右薰衣草)。

**字体(英文)**:
- 显示:`Outfit`(700/800)
- 正文:`Outfit`(400/500)

**字体(中文)**:
- 显示:`ZCOOL KuaiLe 站酷快乐体` — 圆润有童趣,匹配 Outfit 的圆角几何
- 正文:`Noto Sans SC`(400/500)
- `font-family: "Outfit", "ZCOOL KuaiLe", "Noto Sans SC", system-ui, sans-serif;`

**配色**:
```css
:root {
    --bg-peach: #f5e6dc;
    --bg-lavender: #e4dff0;
    --text-dark: #1a1a1a;
    --badge-mint: #c8f0d8;
    --badge-yellow: #f0f0c8;
    --badge-pink: #f0d4e0;
}
```

**标志性元素**:
- 双色背景切分
- 俏皮的图标药丸
- 右侧网格点阵叠层
- 圆角 CTA 按钮

---

### 8. Vintage Editorial

**调性**:有趣、自信、编辑、有人格

**版式**:奶油底 + 居中内容,抽象几何形作点缀。

**字体(英文)**:
- 显示:`Fraunces`(700/900)— 风格鲜明的复古衬线
- 正文:`Work Sans`(400/500)

**字体(中文)**:
- 显示:`Noto Serif SC`(900) 或 `ZCOOL XiaoWei 站酷小薇` — 后者细瘦灵巧,跟 Fraunces 的灵动感更契合
- 正文:`Source Han Sans SC`(500)或 `LXGW Bright`(Regular,增加编辑书卷气)
- `font-family: "Fraunces", "Noto Serif SC", "Songti SC", serif;`(显示)
- `font-family: "Work Sans", "Noto Sans SC", "PingFang SC", system-ui, sans-serif;`(正文)

**配色**:
```css
:root {
    --bg-cream: #f5f3ee;
    --text-primary: #1a1a1a;
    --text-secondary: #555;
    --accent-warm: #e8d4c0;
}
```

**标志性元素**:
- 抽象几何形(圆环 + 直线 + 点)
- 粗描边 CTA 框
- 有点儿"嘴贱"的文案语气
- **不画插图——只用 CSS 几何**

---

## 专题主题

### 9. Neon Cyber

**调性**:未来感、科技、自信

**字体(英文)**:`Clash Display` + `Satoshi`(Fontshare)

**字体(中文)**:
- 显示:`Smiley Sans 得意黑`(Oblique)— 粗壮倾斜,与 Clash Display 的硬朗未来感同频
- 正文:`Sarasa Gothic SC` 或 `HarmonyOS Sans SC`
- `font-family: "Clash Display", "Smiley Sans", "Noto Sans SC", system-ui, sans-serif;`

**配色**:深海军蓝(#0a0f1c)、青色(#00ffcc)、品红(#ff00aa)

**标志性**:粒子背景、霓虹辉光、网格图案

---

### 10. Terminal Green

**调性**:开发者审美、黑客感

**字体(英文)**:`JetBrains Mono`(等宽)

**字体(中文)**:
- `Sarasa Mono SC 更纱等宽`(Regular)— **专为 CJK + 等宽场景而生**,字宽和 JetBrains Mono 完美对齐,中英混排不错位
- `font-family: "JetBrains Mono", "Sarasa Mono SC", "Source Han Mono SC", monospace;`

**配色**:GitHub dark(#0d1117)、终端绿(#39d353)

**标志性**:扫描线、闪烁光标、代码语法高亮

---

### 11. Swiss Modern

**调性**:简洁、精确、包豪斯

**字体(英文)**:`Archivo`(800) + `Nunito`(400)

**字体(中文)**:
- `Noto Sans SC` 单一字体,以 weight 切换(900 / 400)实现层级 — 与瑞士国际主义"用同一字体不同字重"的方法论一致
- `font-family: "Archivo", "Noto Sans SC", "PingFang SC", system-ui, sans-serif;`

**配色**:纯白、纯黑、强调红(#ff3300)

**标志性**:可见网格、不对称版式、几何图形

---

### 12. Paper & Ink

**调性**:编辑、文学、思辨

**字体(英文)**:`Cormorant Garamond` + `Source Serif 4`

**字体(中文)**:
- 显示:`Noto Serif SC`(700)— 现代宋体,匹配 Cormorant 的古典骨架
- 正文:`LXGW WenKai 霞鹜文楷` — 楷书柔笔,把"读书"的体感拉满
- `font-family: "Cormorant Garamond", "Noto Serif SC", "Songti SC", serif;`(显示)
- `font-family: "Source Serif 4", "LXGW WenKai", "Songti SC", serif;`(正文)

**配色**:暖奶白(#faf9f7)、炭黑(#1a1a1a)、酒红(#c41e3a)

**标志性**:首字下沉、引言块、雅致水平分割线

---

## 中文原生预设(无英文对应,专为中文场景)

### 13. 宣纸水墨 / Xuan Paper

**调性**:东方、传统、考究、笔触感

**版式**:米黄宣纸底 + 大字楷书标题 + 朱砂印章 + 局部水墨晕染。

**字体(中文为主)**:
- 显示:`Ma Shan Zheng 马善政楷书` 或 `LXGW WenKai 霞鹜文楷`(Bold)— 题字感,有笔锋
- 正文:`LXGW WenKai 霞鹜文楷`(Regular)
- 数字 / 拉丁:`"Cormorant Garamond"` 作罗马字回退(衬线骨架不抢戏)
- `font-family: "Ma Shan Zheng", "LXGW WenKai", "Songti SC", "STSong", serif;`(显示)
- `font-family: "LXGW WenKai", "Songti SC", serif;`(正文)

**配色**:
```css
:root {
    --bg-paper: #f3ead3;       /* 宣纸黄 */
    --bg-paper-deep: #e8dcb8;  /* 老宣纸 */
    --ink-black: #1a1714;      /* 焦墨 */
    --ink-soft: #4a3f37;       /* 淡墨 */
    --vermilion: #c0392b;      /* 朱砂 */
    --seal-red: #a32c1f;       /* 印章红 */
    --gold-leaf: #b8924d;      /* 描金 */
}
```

**标志性元素**:
- 用 CSS `filter: url(#turbulence)` 或纸张噪点 SVG 模拟宣纸纹理
- **朱砂印章**:CSS 圆角方块 + 篆体名字(用 `Ma Shan Zheng` 或 `Long Cang`),旋转 -3°~5° 错落贴在右下角
- **水墨晕染**:`radial-gradient` 配 `filter: blur(40px)` 制造墨色化开
- **直排标题**(可选,大标题用 `writing-mode: vertical-rl`)
- 横线用 `linear-gradient` 模拟毛笔起收笔的渐淡两端
- **不用任何渐变色文字 / 玻璃拟态 / 投影**,守住"纸 + 墨"两件套

---

### 14. 港式霓虹 / Kowloon Neon

**调性**:夜色、霓虹、街招、复古港风

**版式**:深紫黑底 + 大字粗衬线招牌 + 霓虹勾边 + 局部繁体或印刷感符号。

**字体(中文为主)**:
- 显示:`Noto Serif HK`(900)— 港式宋体专版,招牌字感
- 备选显示:`ZCOOL QingKe HuangYou` 作霓虹粗体
- 正文:`Noto Sans HK`(500)
- `font-family: "Noto Serif HK", "Noto Serif SC", "Songti SC", serif;`(显示)
- `font-family: "Noto Sans HK", "Noto Sans SC", "PingFang HK", system-ui, sans-serif;`(正文)

**配色**:
```css
:root {
    --bg-night: #0a0410;        /* 夜紫黑 */
    --bg-asphalt: #1a0e1f;
    --neon-pink: #ff3d8b;
    --neon-cyan: #00e5ff;
    --neon-yellow: #ffd23f;
    --neon-magenta: #d72ce6;
    --text-light: #faf6e8;       /* 暖白 */
}
```

**标志性元素**:
- **霓虹勾边**:`text-shadow: 0 0 4px var(--neon-pink), 0 0 16px var(--neon-pink), 0 0 28px var(--neon-pink);` 双色叠加更地道
- **招牌底框**:粗描边矩形 + 4 角小圆点(模拟招牌固定钉)
- **闪烁动画**:对一个字符做 `animation: flicker 3s infinite;`(模拟坏掉一格的霓虹管)
- **斜置元素**:整块招牌带 `transform: rotate(-2deg);` 制造街招随手挂的感觉
- 拉丁数字配 `JetBrains Mono` 或 `Space Mono`,带霓虹荧光
- **慎用紫渐变**:即便是港风,渐变也只走"深紫 → 暗酒红",不走"紫 → 蓝"

---

### 15. 杂志感 CN / Editorial CN

**调性**:沉稳、可信、有秩序、有人格

**版式**:大留白 + 巨大宋体标题 + 横线分隔 + 引言块 + 局部点状红/钴蓝。中文场景的"沉稳/可信"首选,替代过于英文味的 Vintage Editorial。

**字体(中文为主)**:
- 显示:`Noto Serif SC`(900,Black)— 极重宋体,做封面式大标题
- 副标题 / 正文:`Noto Sans SC`(400/500)做无衬正文,跟衬线大标形成对比
- 引用:`LXGW WenKai 霞鹜文楷`(Italic)— 区别于正文的笔触,做 pull quote
- `font-family: "Noto Serif SC", "Source Han Serif SC", "Songti SC", serif;`(显示)
- `font-family: "Noto Sans SC", "PingFang SC", "Hiragino Sans GB", system-ui, sans-serif;`(正文)
- `font-family: "LXGW WenKai", "Songti SC", serif; font-style: italic;`(引用)

**配色**:
```css
:root {
    --bg-paper: #f8f6f1;         /* 杂志纸白 */
    --ink-primary: #15110d;      /* 印刷黑 */
    --ink-secondary: #6b6258;
    --accent-vermilion: #d63828; /* 局部点缀红 */
    --accent-cobalt: #1a4d9e;    /* 钴蓝 */
    --rule-color: #15110d;
}
```

**标志性元素**:
- **巨字标题**:`font-size: clamp(3rem, 12vw, 9rem); font-weight: 900; line-height: 0.95;` 字与字几乎咬合
- **细横线分隔**:`border-top: 1px solid var(--rule-color);` 比段落更长,延伸到版心外
- **页码 / 期号**:右上角用 `Sarasa Mono SC` 排成期刊页眉
- **引言块**:左侧粗竖线 + `LXGW WenKai` 斜体,字号 1.5em
- **首字下沉**:`p::first-letter { font-size: 4em; float: left; font-family: "Noto Serif SC"; }`
- **局部颜色锚点**:整页只允许 1–2 个红色或钴蓝色元素(序号、icon、强调词),其余全部黑白

---

## 字体配对速查

| 预设              | 英文显示字体             | 英文正文字体           | 中文显示字体              | 中文正文字体          |
| ----------------- | ------------------------ | ---------------------- | ------------------------- | --------------------- |
| Bold Signal       | Archivo Black            | Space Grotesk          | Smiley Sans               | Noto Sans SC          |
| Electric Studio   | Manrope                  | Manrope                | HarmonyOS Sans SC         | HarmonyOS Sans SC     |
| Creative Voltage  | Syne                     | Space Mono             | ZCOOL QingKe HuangYou     | Sarasa Mono SC        |
| Dark Botanical    | Cormorant                | IBM Plex Sans          | Noto Serif SC             | LXGW WenKai           |
| Notebook Tabs     | Bodoni Moda              | DM Sans                | Noto Serif SC(Black)     | Noto Sans SC          |
| Pastel Geometry   | Plus Jakarta Sans        | Plus Jakarta Sans      | Maoken Tangyuan           | Noto Sans SC          |
| Split Pastel      | Outfit                   | Outfit                 | ZCOOL KuaiLe              | Noto Sans SC          |
| Vintage Editorial | Fraunces                 | Work Sans              | Noto Serif SC / ZCOOL XW  | LXGW Bright           |
| Neon Cyber        | Clash Display            | Satoshi                | Smiley Sans               | Sarasa Gothic SC      |
| Terminal Green    | JetBrains Mono           | JetBrains Mono         | Sarasa Mono SC            | Sarasa Mono SC        |
| Swiss Modern      | Archivo                  | Nunito                 | Noto Sans SC(900)        | Noto Sans SC(400)    |
| Paper & Ink       | Cormorant Garamond       | Source Serif 4         | Noto Serif SC             | LXGW WenKai           |
| **宣纸水墨**      | —                        | (Cormorant Garamond 备用) | Ma Shan Zheng / LXGW WenKai | LXGW WenKai           |
| **港式霓虹**      | —                        | (Space Mono 备用)       | Noto Serif HK             | Noto Sans HK          |
| **杂志感 CN**     | —                        | (Inter / Work Sans 备用) | Noto Serif SC(Black)     | Noto Sans SC          |

---

## 禁用清单(AI 流水线感)

**字体**:Inter、Roboto、Arial、system-ui 当显示字体;微软雅黑、宋体直接套用;思源中等 weight 单调铺满

**配色**:`#6366f1`(俗气 indigo)、白底紫渐变、彩虹色卡片堆叠

**版式**:一律居中、英雄区 + 三卡片网格、一律 12 列对齐

**装饰**:写实插画、不分场景的玻璃拟态、无意义投影、给中文加阴影模拟"印刷"

---

## CSS 陷阱

### 对 CSS 函数取负

**错 — 浏览器静默丢弃,不报错**:
```css
right: -clamp(28px, 3.5vw, 44px);   /* 浏览器丢弃整条声明 */
margin-left: -min(10vw, 100px);      /* 同上 */
```

**对 — 用 `calc()` 包起来**:
```css
right: calc(-1 * clamp(28px, 3.5vw, 44px));
margin-left: calc(-1 * min(10vw, 100px));
```

CSS 不允许函数名前直接出现负号。浏览器会**静默**丢掉整条声明 — 没有报错,元素只是位置不对。**任何对 CSS 函数取负的场景,一律 `calc(-1 * ...)`**。

### 中文字体引用大小写

```css
/* 错 — 部分浏览器对带空格的字体名严格匹配 */
font-family: noto sans sc;

/* 对 — 字面量大小写完全照搬,加引号 */
font-family: "Noto Sans SC", "PingFang SC", system-ui, sans-serif;
```

### 中英混排字距

启用 `font-feature-settings: "palt";` 让汉字按比例宽度排,中文标点不会和拉丁字母之间留出过宽的"空气"。详情见 [animation-patterns.md](animation-patterns.md#中文排版规范)。
