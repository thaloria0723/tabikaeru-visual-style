# CSS 落地参考

> 本文的每个数值都按 SKILL.md「规矩一」标注来源。**你的项目里所有色值都应是【自定】，不是照抄下面的示例值。**

## 0. 变量结构

按 SKILL.md 步骤②的五个角色建变量。角色比例是**结构**，可以照搬；具体色值是**示例**，必须自己生成。

```css
:root {
  /* ① 暖中性底色 —— 占画面最大面积，明度 0.80–0.90 / 饱和 0.15–0.25 */
  --paper-hi:   #F2ECD8;   /* 示例·【自定】亮部 */
  --paper:      #E6DFC4;   /* 示例·【自定】主底色 */
  --paper-lo:   #D9D0B0;   /* 示例·【自定】分层用 */

  /* ② 冷中性对比色 —— 几乎无彩 S<0.10 */
  --cool:       #C2C8CC;   /* 示例·【自定】 */
  --cool-lo:    #A8B0B6;   /* 示例·【自定】 */

  /* ③ 记忆色 —— 角色/品牌色，饱和 0.45–0.60，占屏极小 */
  --brand:      #8A6A4E;   /* 示例·【自定】 */
  --brand-hi:   #A8836A;   /* 示例·【自定】受光面 */

  /* ④ 墨线 —— 全项目共用一支，明度 0.35–0.45 / 饱和 0.10–0.25，非黑非棕 */
  --ink:        #4E5A54;   /* 示例·【自定】 */
  --ink-ui:     #5A4A34;   /* 示例·【自定】木质底材上的文字专用 */

  /* ⑤ 点缀 —— 各占屏 <2% */
  --accent-a:   #E8A07C;   /* 示例·【自定】 */
  --accent-b:   #D9A83C;   /* 示例·【自定】 */

  /* UI 软投影 —— 唯一允许的投影，只用于 UI 面板 */
  --ui-shadow:  0 3px 10px rgb(78 90 84 / 0.22);
}
```

**验证**：把生成的全部色值转成 HSV，检查
- 明度中位数 ≥ 0.75
- 饱和中位数 ≤ 0.35
- `--ink` 明度落在 0.35–0.45
- 没有任何一个值明度 < 0.30（**接近纯黑的都要筛掉**）

---

## 1. 平涂硬边（最重要的一条）

```css
/* ✅ 平涂 */
.surface { background: var(--paper); }

/* ✅ 需要体积时，两档平涂 + 硬边分界 */
.surface--shaded {
  background: var(--paper-lo);
  box-shadow: inset 0 -8px 0 var(--paper);  /* 硬边，不是模糊 */
}
```

### ❌ 禁止清单（CSS 形态）

```css
/* ❌ 1. 任何渐变——全画面禁止（唯一例外：表达「另一种媒介」的图，如照片） */
.panel { background: linear-gradient(#fff, #eee); }
.panel { background: radial-gradient(...); }

/* ❌ 2. 任何纹理/噪点叠加层 —— 实测原作平坦区 SD=0.00 */
body::after {
  background-image: url("noise.png");   /* 或 SVG feTurbulence */
  opacity: .05;
}

/* ❌ 3. 纯黑 / 纯白 —— 实测全场零纯黑像素 */
.ink { color: #000; }
.ink { stroke: #000; }

/* ❌ 4. 弹窗背景压暗遮罩 —— 原作不加 */
.modal-backdrop { background: rgb(0 0 0 / .5); }
.modal-backdrop { backdrop-filter: blur(4px); }

/* ❌ 5. 给插画元素加投影 —— 只有 UI 面板可以 */
.character { filter: drop-shadow(0 4px 6px rgb(0 0 0 / .3)); }

/* ❌ 6. 暗角 / 景深模糊 */
.vignette { box-shadow: inset 0 0 120px rgb(0 0 0 / .4); }
```

---

## 2. 单墨线系统

插画层的勾线走 **SVG**，不走 CSS border（border 拿不到圆头线端）。

```css
.illustration path,
.illustration circle,
.illustration rect {
  fill: none;
  stroke: var(--ink);        /* 全项目一支墨，不随填充变化 */
  stroke-width: 2;           /* 【推导】1.3–2.7pt @3x */
  stroke-linecap: round;     /* 圆头线端 */
  stroke-linejoin: round;    /* 圆角转角 */
  vector-effect: non-scaling-stroke;
}

/* ✅ 同一支墨色，粉底白底都一样 */
.illustration .petal  { fill: var(--accent-a); }
.illustration .plate  { fill: #FFFFFF; }       /* 纯白平涂是允许的填充，只是不能做线和字 */
```

> **注意**：禁的是**纯黑**（`#000` 做线色/字色），**纯白做填充**是允许的——原作商店标签板就是 `#FFFFFF` 平涂。

### UI 层的线

UI 面板若需要边框，用**同一个 `--ink`**，不要另起一套：

```css
.panel--outlined { border: 2px solid var(--ink); border-radius: 12px; }
```

---

## 3. UI 软投影分层

**美术层无投影，UI 层有软投影。** 这个分层是刻意的，别搞混。

```css
/* ✅ UI 面板浮起来 */
.panel--floating {
  background: var(--paper-hi);
  border-radius: 12px;
  box-shadow: var(--ui-shadow);
}

/* ✅ 大弹窗用大面积平涂 + 软投影，不用遮罩 */
.sheet {
  background: var(--cool);        /* 示例：【自定】大面积单色底 */
  border-radius: 16px;
  box-shadow: var(--ui-shadow);
}
```

```html
<!-- ❌ 不要这样 -->
<div class="modal-backdrop"></div>
<div class="sheet">…</div>

<!-- ✅ 这样 -->
<div class="sheet">…</div>
```

**弹窗打开时，背后的场景保持全亮度。** 这是维持全画面高明调的关键。

---

## 4. 文字

### 4.1 标题字（袋文字）

原作的标题是**手绘字，不是字体**。CSS 只能近似——**要诚实标注这是近似，不要声称还原。**

```css
.title {
  font-family: var(--font-title);
  -webkit-text-stroke: 0.12em var(--ink-ui);  /* 【自定】描边宽度——必须用 em */
  paint-order: stroke fill;                   /* 关键：让描边压在填充下面 */
  color: var(--paper-hi);                     /* 浅色内芯 */
  letter-spacing: .04em;
}
```

> ⚠️ **描边宽度必须用 `em`，不要写死 `px`。** 写 `6px` 配 40rpx（≈20px）的字号，描边会吃掉整个字形糊成一团——而读者没有理由怀疑一个看起来正常的数字。实测观察到的失败模式：读者会直接照抄技能里的示例值。示例值本身出错比不给示例更危险。
>
> `0.12em` 是【自定】起点值，**不是测出来的**——原作标题是手绘字，描边比例未经测量。请按你的字号实测调整。
>
> `paint-order: stroke fill` 是这条能成立的关键；不写它描边会吃掉字形。

### 4.2 字体栈

**【事实·来源】中文字体选型**（授权信息经官方 LICENSE / OFL 原文核验）

**首推：标题／氛围字用「霞鹜文楷」，正文用系统栈。**

```css
:root {
  /* 标题 / 氛围字 —— 霞鹜文楷（OFL 1.1） */
  --font-title: "LXGW WenKai", "霞鹜文楷",
                "Songti SC", "Kaiti SC", serif;

  /* 正文 —— 系统栈，气质优先但不勉强 */
  --font-body: -apple-system, "PingFang SC", "HarmonyOS Sans SC",
               "Noto Sans CJK SC", "Source Han Sans SC", sans-serif;

  /* 数字 / 拉丁 —— Ysabeau Office（霞鹜文楷作者官方推荐搭配） */
  --font-num: "Ysabeau Office", var(--font-body);
}
```

**为什么是霞鹜文楷**：
- 基于 FONTWORKS Klee One，**楷体笔调 + 仿宋端正**，天然是「书卷气」而不是「可爱」
- 授权是 **OFL 1.1**，且其 OFL.txt 含罕见的 **ADDITIONAL PERMISSION**：明文授权「为网页加载而子集化 / 转 WOFF2 可继续使用保留名称」，条件只是不得作为可安装桌面字体分发
- 有官方 npm webfont 包和国内 CDN

**⚠️ 同系列不同授权的大坑**：**霞鹜新晰黑是 IPA Font License 1.0，不是 OFL**。嵌入条款需单独评估（其 README 自己也这么警告）。**不要以为「霞鹜系列都是 OFL」。**

**⚠️ 授权上需要你自行确认的**：
- **京華老宋体**：作者声明免费商用 vs 字体元数据 `All rights reserved` vs 部分站点称付费——**三方冲突，建议避开**，要用就联系作者书面确认
- **全瀨體**：官方授权页未找到，第三方说的 OFL 无法证实
- **朱雀仿宋**（预览版约 6763 字）、**辰宇落雁體**（约 9000 字，简体弱）**可能缺字**，需用你的实际文案跑一遍
- **中文网字计划 CDN**：实测可用（自带 269 个 @font-face 切片，单片约 55KB），但**域名历史上迁移过**、公益项目无 SLA，正式发布建议自建子集

### 4.3 加载方案（中文 Web 字体的核心难点）

**【实测】中文字体 24–27 MB / 字重**（霞鹜文楷 Regular 24.39 MB）。整包加载在移动端**不可行**。

**唯一可行路线：子集化 + 只给标题用 Web 字体，正文走系统栈。**

```css
/* 只给标题加载，且用 unicode-range 切片按需取 */
@font-face {
  font-family: "LXGW WenKai";
  src: url("./fonts/lxgw-subset.woff2") format("woff2");
  unicode-range: U+4E00-9FFF, U+3000-303F;   /* 按实际用字裁剪 */
  font-display: swap;
  font-weight: 400;
}
```

**⚠️ Google Fonts 只有繁体版 `LXGW WenKai TC`**，简体 `LXGW WenKai` / `GB` 在 Google Fonts 返回 400，且国内访问不可靠。**别指望 Google Fonts。**

### 4.4 微信小程序（如走小程序路线）

**小程序不能用 CSS `@font-face` 加载网络字体**，必须用 `wx.loadFontFace`：

```js
// app.js
wx.loadFontFace({
  family: 'LXGW WenKai',
  source: 'url("https://你的域名/lxgw-subset.ttf")',
  global: true,          // 全局生效需基础库 2.10.0+
  scopes: ['webview', 'native'],
});
```

**【事实·来源】小程序字体格式坑**：
- 官方明确建议 **TTF / WOFF**；**WOFF2 在低版本 iOS 不兼容**
- `source` 域名须 **https 且同源/CORS**（小程序域名为 `servicewechat.com`）
- 静态资源域名需在小程序后台配置

### 4.5 纯系统栈（不加载 Web 字体）

```css
--font-system: "Songti SC", "Kaiti SC", "STKaiti",
               "Noto Serif CJK SC", "Source Han Serif SC", serif;
```

**⚠️ 硬伤（【事实·来源】）**：iOS 系统可拿到 `Songti SC` / `Kaiti SC`（有书卷气），但 **Android 基本只有黑体**。**纯系统栈两端气质不统一**——这是跳过 Web 字体的直接代价，得认。

### 4.6 反面清单：这些字体不能用

**圆体整体禁用**——圆润甜美的气质与本风格直接冲突：

| 禁用 | 原因 |
|---|---|
| 华康少女体 | 太甜 |
| 站酷快乐体 | 太闹 |
| 郑庆科黄油体 | 太圆太软 |
| **iOS 的 `Wawati SC`（娃娃体）** | 系统自带但气质不对，别顺手拿来用 |

**⚠️ 清松手写体是同名不同气质的陷阱**：

| 编号 | 气质 | 可用性 |
|---|---|---|
| 1 | 圆润 | ❌ 不能用 |
| 3 | 呆萌 | ❌ 不能用 |
| 6 | Q 萌 | ❌ 不能用 |
| **5** | **行楷** | ✅ 可用 |
| **8** | **随性** | ✅ 可用 |
| **9** | **文青** | ✅ 可用 |

**同一个字体家族名前缀，编号不同气质完全相反。** 下载前务必看清编号。

---

## 5. 自检脚本（建议直接写进 CI 或构建流程）

```bash
# 检查是否有被禁的 CSS 写法
rg -n "linear-gradient|radial-gradient|feTurbulence|backdrop-filter" src/ \
  && echo "❌ 发现渐变/噪点/遮罩" 

rg -n "#000|#000000|rgb\(0 0 0|rgba\(0,\s*0,\s*0" src/ \
  && echo "❌ 发现纯黑"
```

> ⚠️ **扫描范围必须限定在 `src/`（或你的实际代码目录）。**
> 实测踩到的坑：如果把文档目录也扫进去，会命中**文档自己在讲「不要这么做」的示例代码**——一次实测里有 9 处误报。门禁一旦开始误报，人就会习惯性忽略它，门禁就废了。

**色板包络检查**：把全部色值转 HSV，断言
- 明度中位 ≥ 0.75
- 饱和中位 ≤ 0.35
- 无值明度 < 0.30
