# 中文网页/小程序字体方案调研
## 项目：上海交大校园探索游戏 · UI 气质对标《旅行青蛙》· 移动端优先

> **本文标注约定**
> - ✅ **已查证** = 我从官方仓库 LICENSE / 官方站点 / 官方文档原文核实到的
> - ⚠️ **不确定** = 来源冲突或无法从官方渠道核实，**用前请自行确认**
> - 💭 **推断** = 我的主观设计判断，非事实
>
> 调研日期：2026-09-12。授权条款可能变更，上线前请复核。

---

## 一、结论速览

**首推方案（三段式）：**

| 层级 | 方案 | 理由 |
|---|---|---|
| 标题 / 氛围字 | **霞鹜文楷 LXGW WenKai**（子集化按需加载） | 楷体骨架 + 仿宋的端正，天然「书卷气」；OFL 1.1 且作者**明文额外授权** Web 子集化 |
| 正文 / UI 长文本 | **系统字体栈**（PingFang SC / HarmonyOS Sans SC / MiSans…） | 楷体不适合小字号长文；正文用系统黑体最稳、零加载 |
| 数字 / 西文 | **Ysabeau Office** 或 **EB Garamond** | Ysabeau 是霞鹜文楷作者本人官方推荐的搭配字体 |

**关键判断：** 中文字体文件 24–27 MB/字重，**不可能整包加载**。移动端唯一可行路线是
**unicode-range 子集化 + 按需加载**，且**只给标题/氛围文案用 Web 字体，正文回退系统字体**。

---

## 二、候选字体对比表

### 2.1 手写 / 楷体系（本项目的核心气质来源）

| 字体名 | 气质 | 适合用途 | 授权 | Web 可用性 | 体积 |
|---|---|---|---|---|---|
| **霞鹜文楷**<br>LXGW WenKai | 楷体骨架带仿宋端正，温润克制，**最贴「书卷气」** | 标题、诗句、旁白、注释、中等长度文本 | ✅ **SIL OFL 1.1**<br>+ 明文额外授权 Web 子集化 | ✅ 有 npm webfont 包 + 国内 CDN | 完整 TTF **24.4 MB**(Regular)<br>26.96 MB(Light)<br>子集切片 avg **43 KB** |
| **芫荽**<br>Iansui | 硬笔楷书，贴近台湾教育部标准字形，比文楷更「工整」 | 标题、教学感文案 | ✅ **SIL OFL 1.1** | ✅ 可自行子集化 | 未实测 |
| **悠哉字体**<br>Yozai | 随性手写，笔画松弛，**「安静」而非「可爱」** | 标题、便签/日记类 UI | ✅ **SIL OFL 1.1** | ✅ 有 CDN 包 | 未实测 |
| **清松手写体 5/8/9**<br>Jason Handwriting | 5=行楷 / 8=随性 / 9=文青 | 标题、手写便签 | ✅ **SIL OFL 1.1** | ✅ 可自行子集化 | 未实测 |
| **辰宇落雁體**<br>Chenyuluoyan | 高中生生徒手写，细瘦、克制、有呼吸感 | 少量氛围文案 | ✅ **SIL OFL 1.1** | ✅ 可自行子集化 | 未实测 |
| **全瀨體**<br>cjkFonts | AI 造字手写体 | 标题 | ⚠️ **不确定**<br>第三方称 OFL 1.1，**未从官方核实** | ⚠️ 未核实 | 未实测 |

### 2.2 宋体系（书卷气最正、最耐读）

| 字体名 | 气质 | 适合用途 | 授权 | Web 可用性 | 体积 |
|---|---|---|---|---|---|
| **思源宋体 / 思源宋體**<br>Source Han Serif SC<br>= Noto Serif SC | 现代宋体，端正清雅，**正文书卷气的安全牌** | 正文、长文本、标题 | ✅ **SIL OFL 1.1** | ✅ Google Fonts（unicode-range 切片）<br>⚠️ Google Fonts 在国内不可靠 | SC 全字重 zip **132 MB**<br>单字重约 15–20 MB 💭 |
| **朱雀仿宋**<br>Zhuque Fangsong | 基于民国活字南宋改造，**仿宋特有的清瘦文气** | 正文、引文、标题 | ✅ **SIL OFL 1.1**<br>（西文部分 Alegreya 同为 OFL） | ✅ 有 CDN 包(`zqfs`) | 未实测 |
| **京華老宋体**<br>KingHwa OldSong | 复刻 1961 年新华字模厂老筑地体，**年代感/铅字味** | 标题、年代氛围 | ⚠️ **不确定，必须自行确认**<br>见 §3.3 | ⚠️ 有 CDN 包(`jhlst`)但**授权未明** | TTF 约 27–35 MB<br>(来源称，未实测) |

### 2.3 圆体系（⚠️ 本项目要**克制使用**——圆体极易滑向「甜腻可爱」）

| 字体名 | 气质 | 适合用途 | 授权 | Web 可用性 | 体积 |
|---|---|---|---|---|---|
| **Yuanti SC**<br>（iOS/macOS 系统圆体） | 苹果圆体，圆而不甜，**系统自带** | 若一定要圆体，用这个最克制 | 系统字体，**无需授权**（仅引用字体名） | ✅ 零加载 | 0 |
| **MiSans**<br>（小米，含圆润字形） | 简约人文，比纯圆体收敛 | 正文/UI | ✅ 官方称「全球免费商用字体」 | ✅ 官网提供 WOFF/WOFF2 | 见官网 |
| **思源柔黑** 等 | 圆黑，日系 | — | 多为 OFL 衍生 | — | — |

> 💭 **推断**：圆体系在本项目里**建议整体放弃**。《旅かえる》的柔和来自「手写 + 留白 + 低饱和」，
> 不是来自圆体。圆体会把气质从「安静」拉到「可爱」。若非要用圆，只用 **Yuanti SC** 且仅用于
> 极少量强调元素。

### 2.4 黑体系（UI 骨架 / 正文兜底）

| 字体名 | 气质 | 适合用途 | 授权 | Web 可用性 | 体积 |
|---|---|---|---|---|---|
| **思源黑体**<br>Source Han Sans SC = Noto Sans SC | 中性现代，无性格但有秩序 | 正文、UI 控件 | ✅ **SIL OFL 1.1** | ✅ Google Fonts 切片 | 大（同思源宋体量级） |
| **更纱黑体**<br>Sarasa Gothic | 思源黑体 + Inter + Iosevka 合并，**等宽/UI 场景强** | 代码、数字表格 | ✅ **SIL OFL 1.1** | ✅ 可自行子集化 | TTF 全家族包 **812 MB**<br>（含所有变体，需自行裁切） |
| **霞鹜新晰黑**<br>LXGW Neo XiHei | 基于 IPAex 黑体改造的国标字形，**比思源黑体更「瘦硬」** | 正文 | ⚠️ **IPA Font License 1.0**<br>**不是 OFL！** 见 §3.2 | ⚠️ 需评估嵌入合规成本 | 未实测 |

---

## 三、授权详解（逐个查证结果）

### 3.1 ✅ OFL 1.1 家族——可放心商用 + Web 嵌入

**SIL Open Font License 1.1 通用规则：**
- ✅ 免费商用，无需付费、无需告知作者
- ✅ 可嵌入 Web / App / 游戏 / 硬件
- ✅ 可修改、可再分发（再分发需附 OFL.txt 全文）
- ❌ **禁止单独售卖字体文件本身**
- ❌ 衍生字体必须继续以 OFL 1.1 发布
- ⚠️ 衍生字体**不得使用「保留名称」（Reserved Font Name）**

**已核实为 OFL 1.1 的字体：**

| 字体 | 版权行（原文） | 保留名称 |
|---|---|---|
| 霞鹜文楷 | `Copyright 2021-2026 LXGW, with Reserved Font Name '霞鹜','霞鶩','落霞孤鹜','落霞孤鶩','LXGW'. Copyright 2020 The Klee Project Authors.` | 霞鹜 / 霞鶩 / 落霞孤鹜 / 落霞孤鶩 / LXGW |
| 芫荽 Iansui | `Copyright 2025 The Iansui Project Authors` | （Google Fonts 讨论中曾涉及 RFN 争议） |
| 悠哉字体 Yozai | `Copyright (C) 2020 LXGW`（原始数据版权归 Y.OzVox） | 悠哉 / Yozai |
| 朱雀仿宋 | `© 智琮科技/璇玑造字` | — |
| 更纱黑体 Sarasa | `Copyright (c) 2015-2025, Renzhi Li. Portions Copyright (c) 2016 The Inter Project Authors. Portions Copyright (c) 2014-2021 Adobe Systems Inc., with Reserved Font Name 'Source'. Portions Copyright (c) 2012 Google Inc.` | Source |
| 辰宇落雁體 | OFL 1.1（GitHub `Chenyu-otf/chenyuluoyan_thin`） | 辰宇落雁 |

**🎯 霞鹜文楷的关键好消息——Web 子集化被明文额外授权：**

OFL 的「保留名称」条款本来是 Web 字体的大坑（子集化 = 修改 = 不能用原字体名）。
但霞鹜文楷的 OFL.txt 里有一条 **ADDITIONAL PERMISSION**，原文：

> `[ADDITIONAL PERMISSION] The Reserved Font Names '霞鹜','霞鶩','落霞孤鹜','落霞孤鶩' and 'LXGW'
> may continue to be used in Modified Versions recompiled from the Original Version, without
> modifications to the font source code; or in Modified Versions subsetted or converted to other
> formats (e.g., WOFF/WOFF2) **solely for web font delivery**, provided such Modified Versions are
> not made available as installable desktop fonts (e.g., on mainstream platforms like Google Fonts,
> or third-party non-commercial platforms recognized by the author @lxgw; other web font platforms
> please contact the author @lxgw for confirmation).`

**也就是说：为网页加载而做子集化 / 转 WOFF2，可以继续叫「霞鹜文楷」，无需另行授权。**
唯一条件是：**不要把子集化后的文件当成可安装桌面字体公开提供下载。**

> 来源：https://raw.githubusercontent.com/lxgw/LxgwWenKai/main/OFL.txt

**其他 OFL 字体的 Web 子集化提醒**：OFL 通用条款下，子集化属于「Modified Version」，
**技术上不应继续使用保留名称**（如「悠哉」「辰宇落雁」）。实践中：
- 若自建子集并改名（如 `MyGame-Kai`），完全合规
- 若想保留原名，需联系作者确认（霞鹜文楷已明文授权，其余未确认）

### 3.2 ⚠️ 霞鹜新晰黑 = IPA Font License 1.0，**不是 OFL**

**这是一个容易踩的坑。**「霞鹜」系列并非全部同授权：

- 霞鹜文楷 / 悠哉字体 → **OFL 1.1**
- **霞鹜新晰黑 → IPA Font License 1.0**（衍生自 IPAexゴシック）

其 README 原文警示：

> `本项目衍生自 IPA 字体，遵循 IPA Font License 1.0。若计划将本项目字体用于嵌入式用途，
> 请务必仔细阅读 IPA Font License 1.0 条款，并参考「嵌入须知」自行评估合规成本。`

**含义**：IPA 许可允许免费商用与再分发，但对**嵌入（embedding）**有额外条款要求，
与 OFL 的宽松程度不同。**如果要用霞鹜新晰黑做 Web 字体，请先读 IPA 1.0 原文并评估。**

> 来源：https://github.com/lxgw/LxgwNeoXiHei
> 嵌入须知：https://github.com/lxgw/lxgw/blob/main/documents/xizhi_embedding_instructions.md

### 3.3 ⚠️ 京華老宋体——**授权最不确定，强烈建议自行确认**

**这是本次调研中唯一一个我无法给出确定结论的字体。**

查证到的**冲突信息**：

| 来源 | 说法 |
|---|---|
| 作者「特里王」公开授权声明（经多个字体站转载） | **免费商用**：可用于平面/包装/宣传/影视/网页，可嵌入电子产品、软件应用；可自由传播；**不可单独出售字体文件**；**禁止修改字形、禁止传播修改版**；**字形不符合任何地区文字规范，切勿用于教育或用字讲究的正规场合** |
| 字体文件内嵌元数据（我从 CDN 上的 result.css 实测） | `Font © Copyright 2022 TerryWang. All rights reserved.` |
| 部分收录站（字加网/方正页面模板、FontHubs FAQ） | 称「付费商用字体，需购买商用授权」/「不可以免费商用」 |

**我的判断（💭 推断）：**
- 作者本人的中文声明是**最有分量的来源**，字加网那条大概率是页面模板残留
- 但**字体元数据写着 "All rights reserved"**，且**没有任何标准开源协议（非 OFL）**
- 因此它**不是开源字体，而是「作者单方声明的免费商用」**

**🚨 建议：本项目（会公开发布的学生作品）若要用京華老宋体，请务必：**
1. 直接联系作者（特里王 / 知乎「活字考古」）取得**书面确认**
2. 保留作者授权声明的截图/存档
3. 或**直接改用朱雀仿宋 / 思源宋体**规避风险

> 另注：作者明确说**字形不符合文字规范，不要用于教育场合**——校园项目或许也算敏感场景。

### 3.4 ✅ 系统字体——**引用字体名不涉及授权**

**重要澄清**：在 `font-family` 里写 `"PingFang SC"`、`"MiSans"` 等**系统字体名**，
**不构成字体嵌入或分发，不需要任何授权**。授权只在你**打包/分发字体文件**时才生效。

同理，**MiSans**（小米）官方页面标注 **"Global Free Commercial-Use Fonts / 全球免费商用字体"**，
并提供 VF/OTF/TTF/WOFF/WOFF2 下载——即它也可作为 Web 字体自托管商用。

> 来源：https://hyperos.mi.com/font/zh/

### 3.5 授权速查小结

| 字体 | 免费商用 | Web @font-face | 需要署名 | 主要坑 |
|---|---|---|---|---|
| 霞鹜文楷 | ✅ 可 | ✅ **明文允许** | 否 | 不可单独卖字体文件 |
| 芫荽 Iansui | ✅ 可 | ✅ 可 | 否 | 繁体为主，简体字汇需确认 |
| 悠哉字体 | ✅ 可 | ✅ 可 | 否 | 子集化后建议改名 |
| 清松手写体 | ✅ 可 | ✅ 可 | 否 | 简繁支持因版本而异 |
| 辰宇落雁體 | ✅ 可 | ✅ 可 | 否 | 简体较弱，可能缺字 |
| 思源宋/黑体 | ✅ 可 | ✅ 可 | 否 | 无 |
| 朱雀仿宋 | ✅ 可 | ✅ 可 | 否 | 仍是预览测试版，字汇有限 |
| 更纱黑体 | ✅ 可 | ✅ 可 | 否 | 体积巨大需自裁 |
| **霞鹜新晰黑** | ✅ 可 | ⚠️ **IPA 1.0 需评估** | 否 | **非 OFL** |
| **京華老宋体** | ⚠️ **不确定** | ⚠️ **不确定** | ⚠️ | **非开源协议，All rights reserved** |
| **全瀨體** | ⚠️ **未核实** | ⚠️ **未核实** | ⚠️ | 未找到官方授权页 |

---

## 四、Web 加载方案（移动端 H5 / 小程序）

### 4.1 核心矛盾：中文字体有多大（实测数据）

| 对象 | 实测体积 |
|---|---|
| 霞鹜文楷 Regular 完整 TTF | **24.39 MB** |
| 霞鹜文楷 Light 完整 TTF | **26.96 MB** |
| 霞鹜文楷 Medium 完整 TTF | **24.20 MB** |
| 思源宋体 SC 全字重 zip | **132.21 MB** |
| 更纱黑体 TTF 全家族包 | **812.35 MB** |

**结论：任何「整包加载」方案在移动端都不可行。**

### 4.2 可行方案：unicode-range 子集化（实测数据）

原理：把字体按 Unicode 区段切成几十~几百个小 woff2 切片，
CSS 用 `unicode-range` 声明每片的覆盖范围，**浏览器只下载页面实际用到的字所在的那几片**。

**实测数据（霞鹜文楷 Lite，npm 包 `lxgw-wenkai-lite-webfont@1.7.0`）：**
- 共 **595 个文件**，总 **25.13 MB**
- woff2 切片 **582 个**，平均 **43.1 KB**，最大 **71.8 KB**，最小 **7.0 KB**
- → 一个典型页面通常只命中 **3–8 片**，实际下载量约 **150–350 KB** 💭

**实测数据（中文网字计划 CDN，LXGW WenKai Regular）：**
- `result.css`：**192 KB**，**269 个** `@font-face` 块，共 17,400 条 `unicode-range`
- 单片 woff2：**56.7 KB**（实测 `363d78ac...woff2`）

**实测数据（Google Fonts）：**
- Noto Serif SC 单字重：**101 片**
- LXGW WenKai TC 单字重：**115 片**
- 现代 UA 返回 woff2 + unicode-range；**老 UA 会返回完整 TTF**（巨坑）

### 4.3 各方案代价对比

| 方案 | 代价 | 适用 |
|---|---|---|
| **系统字体回退栈** | 零加载；但 Android 拿不到宋/楷，气质不统一 | ✅ **正文/UI 必用** |
| **unicode-range 子集化（自建）** | 需跑 `cn-font-split`/`fonttools` 构建流程；产物文件多 | ✅ 推荐 |
| **中文网字计划 CDN** | 免费、开箱即用、国内可访问；但**依赖第三方、域名历史上迁移过** | ✅ 推荐（原型/小项目） |
| **Google Fonts** | 免费、自动切片；**国内不可靠访问**；**只有繁体版霞鹜文楷** | ❌ 国内项目不推荐 |
| **按需加载（JS 检测用字后加载）** | 逻辑复杂，首屏可能字体闪烁 (FOUT) | ⚠️ 过度设计 |
| **整包 TTF/WOFF2** | 24 MB+，移动端不可接受 | ❌ |
| **base64 内嵌** | 体积膨胀 ~33%，且无法缓存 | ❌ 仅适合极小图标字体 |

### 4.4 中文网字计划（推荐）

**✅ 已查证**：一套开源的全场景 Web 中文字体方案，提供免费 CDN + 分包引擎。

- 官网：https://chinese-font.netlify.app/
- 字体仓库（80 个包）：https://github.com/KonghaYao/chinese-free-web-font-storage
- 分包引擎：https://github.com/KonghaYao/cn-font-split
- Vite 插件：`vite-plugin-font`

**CDN 用法（实测可用）：**
```html
<link rel="stylesheet"
  href="https://cn-font.claude-code-best.win/packages/lxgwwenkai/dist/LXGWWenKai-Regular/result.css">
```

URL 模式：`/packages/{包名}/dist/{字体名-字重}/result.css`

**实测确认存在的包**（共 80 个，摘录相关）：

| 包名 | 字体 |
|---|---|
| `lxgwwenkai` | 霞鹜文楷 |
| `lxgwwenkaibright` | 霞鹜文楷 Bright（与 Ysabeau 合并西文） |
| `lxgwmanhei` | 霞鹜漫黑 |
| `yozai` | 悠哉字体 |
| `jhlst` | 京華老宋体 ⚠️ 授权不明 |
| `zqfs` | 朱雀仿宋 |
| `LxgwNeoZhiSong` | 霞鹜新致宋 |
| `maple-mono-cn` | Maple Mono CN |

**⚠️ 注意事项：**
1. CDN 有 **Referer 防盗链**，curl 直接访问返回 403（浏览器 `<link>` 加载正常）
2. CDN 域名**历史上迁移过**（`chinese-fonts-cdn.deno.dev` → `cn-font.claude-code-best.win`），
   官网明确提示旧域名即将退役 → **上线前请复核当前域名**
3. 该 CDN 是**公益性质第三方服务**，无 SLA。**正式发布建议自建**（用 `cn-font-split` 自己切）

### 4.5 微信小程序特殊处理（关键差异）

**🚨 小程序不能用 CSS `@font-face` 加载网络字体**，必须用 JS API `wx.loadFontFace`。

**✅ 已查证的官方文档要点**（来源：微信开放文档 `wx.loadFontFace`）：

参数：
| 参数 | 类型 | 必填 | 说明 | 最低版本 |
|---|---|---|---|---|
| `global` | boolean | 否 | 是否全局生效 | 2.10.0 |
| `family` | string | **是** | 定义的字体名称 | — |
| `source` | string | **是** | 字体资源地址，可为 **https 链接或 Data URL** | Data URL 需 3.7.9+ |
| `desc` | Object | 否 | 可选字体描述符 | — |
| `scopes` | Array | 否 | 作用范围：`webview`/`native`/`skyline`，默认全选；设 `native` 可在 Canvas 2D 用 | 3.7.9 前默认 `webview` |
| `success`/`fail`/`complete` | function | 否 | 回调 | — |

**官方 Tips（原文要点）：**
1. 字体链接**需要是下载类型**
2. `content-type` 需符合 IANA font 媒体类型，**格式不正确会解析失败**
3. **字体链接必须是 https（iOS 不支持 http）**
4. **建议格式为 TTF 和 WOFF；WOFF2 在低版本 iOS 上会不兼容** ← 重要！
5. 字体链接必须**同源或开启 CORS**，小程序的域名是 `servicewechat.com`
6. 开发者工具提示 `Faild to load font` 可以忽略
7. **2.10.0 以前仅在调用页面生效；2.10.0 起支持全局生效，需在 `app.js` 中调用**

**示例代码（官方）：**
```js
wx.loadFontFace({
  family: 'Bitstream Vera Serif Bold',
  source: 'url("https://res.wx.qq.com/t/wx_fed/base/weixin_portal/res/static/font/33uDySX.ttf")',
  success: console.log
})
```

**💭 小程序场景推断建议：**
- 小程序的字体分包能力弱于 H5，**建议小程序端只用系统字体**，或
- 若必须用霞鹜文楷：用 TTF/WOFF（**不要 WOFF2**）、走 `app.js` 全局加载、**只加载一个子集**
  （把游戏里所有会出现的汉字先提取出来做一个静态子集，可能只有几百 KB）

### 4.6 🎯 移动端 H5 场景的推荐做法

```
第 1 步：正文/UI 一律用系统字体栈（零加载，见 §5）
第 2 步：仅标题、诗句、旁白、按钮文案用霞鹜文楷
第 3 步：用 unicode-range 子集化加载（中文网字计划 CDN 起步 / 后期自建）
第 4 步：font-display: swap 防止阻塞渲染
第 5 步：字号小于 16px 的地方一律不用楷体（笔画糊）
第 6 步：首屏关键文字可考虑用图片/SVG 兜底，避免 FOUT 抖动
```

**备选极简方案（零风险、气质打折）：** 完全放弃 Web 字体，
仅在 iOS 上用系统「宋体/楷体」栈（见 §5），接受 Android 上是黑体。

---

## 五、系统字体回退栈

### 5.1 各平台默认中文字体（✅ 已查证 iOS/macOS；Android 部分为 💭 推断）

**iOS / macOS**（来源：Apple 官方系统字体列表 https://developer.apple.com/fonts/system-fonts/）

| 类别 | 可用字体名（官方原文） |
|---|---|
| **默认无衬线** | `PingFang SC`（Ultralight / Thin / Light / Regular / Medium / Semibold） |
| 繁体/港/澳 | `PingFang TC` / `PingFang HK` / `PingFang MO` |
| **宋体** | `Songti SC`（Light / Regular / Bold / Black）、`Songti TC`、`STSong` |
| **楷体** | `Kaiti SC`（Regular / Bold / Black）、`Kaiti TC`、`STKaiti` |
| **圆体** | `Yuanti SC`（Light / Regular / Bold）、`Yuanti TC` |
| 手写 | `Hannotate SC`、`HanziPen SC`（翩翩体）、`Xingkai SC`（行楷） |
| 其他 | `Heiti SC`、`Hiragino Sans GB W3/W6`、`Lantinghei SC`、`Baoli SC`、`Libian SC`、`Wawati SC` |

**Android**（💭 推断，厂商 ROM 差异大，无法穷举）
- 原生/AOSP：`Noto Sans CJK SC`（= 思源黑体）
- 部分设备有 `Noto Serif CJK SC`（= 思源宋体），但**不保证**
- 厂商定制：`HarmonyOS Sans SC`（华为）、`MiSans`（小米）、`OPPO Sans`、`vivo Sans`
- ⚠️ **Android 上基本拿不到楷体/宋体**，这是纯系统方案的硬伤

**微信小程序**
- 小程序 WebView 内即系统 WebView，**默认继承系统字体**
- iOS → PingFang SC；Android → 各家系统黑体
- 想统一必须走 `wx.loadFontFace`（见 §4.5）

### 5.2 直接可用的 `font-family` 声明

```css
/* ============================================================
   上海交大校园探索游戏 · 字体栈
   气质：柔和 / 克制 / 手写感 / 书卷气
   ============================================================ */

:root {
  /* ---------- A. 标题 / 氛围文案（已按需加载霞鹜文楷） ---------- */
  --font-title:
    "LXGW WenKai", "霞鹜文楷", "LXGW WenKai Screen",
    "Songti SC", "STSong", "Kaiti SC", "STKaiti",   /* iOS/macOS 书卷气回退 */
    "Noto Serif CJK SC", "Source Han Serif SC",      /* Android 若有思源宋 */
    "SimSun", "NSimSun",                             /* Windows */
    serif;

  /* ---------- B. 正文 / UI（可读性优先，零加载） ---------- */
  --font-body:
    "PingFang SC",                                   /* iOS/macOS 默认 */
    "HarmonyOS Sans SC", "MiSans", "OPPO Sans", "vivo Sans",  /* 国产 ROM */
    "Noto Sans CJK SC", "Source Han Sans SC",        /* Android 原生 */
    "Hiragino Sans GB",                              /* 旧 macOS */
    "Microsoft YaHei",                               /* Windows */
    system-ui, -apple-system, sans-serif;

  /* ---------- C. 纯系统「温柔书卷气」方案（不加载任何 Web 字体） ---------- */
  --font-soft-system:
    "Songti SC", "STSong",                           /* iOS 宋体 —— 最书卷气 */
    "Kaiti SC", "STKaiti",                           /* iOS 楷体 —— 更手写感 */
    "Noto Serif CJK SC", "Source Han Serif SC",      /* Android 若有 */
    "SimSun", "NSimSun",
    "PingFang SC",                                   /* 最后兜底 */
    "Microsoft YaHei",
    serif;

  /* ---------- D. 数字 / 拉丁（与柔和中文搭配） ---------- */
  --font-num:
    "Ysabeau Office", "EB Garamond", "Crimson Pro",
    "Iowan Old Style", "Georgia", "Times New Roman", serif;
}

/* ============================================================
   使用建议
   ============================================================ */

/* 标题：霞鹜文楷，字重不要低于 400（Light 太细，小字号会糊） */
.title, h1, h2, .poem, .narration {
  font-family: var(--font-title);
  font-weight: 400;          /* 霞鹜文楷 Regular 实为 Klee SemiBold，视觉已偏细 */
  letter-spacing: 0.02em;    /* 楷体略加字距，呼吸感更强 */
  line-height: 1.6;
}

/* 正文：系统黑体，稳、清晰、零加载 */
body, p, .content {
  font-family: var(--font-body);
  line-height: 1.75;         /* 中文正文 1.7–1.8 易读 */
}

/* 数字/日期/数量：单独指定，避免中文楷体的西文部分不协调 */
.num, .date, .count, time, .en {
  font-family: var(--font-num);
  font-variant-numeric: tabular-nums;   /* 数字等宽，列表对齐 */
  letter-spacing: 0.01em;
}

/* ============================================================
   Web 字体加载（H5）—— unicode-range 子集化，实测单片约 55 KB
   ============================================================ */
@font-face {
  font-family: "LXGW WenKai";
  font-style: normal;
  font-weight: 400;
  font-display: swap;        /* 关键：不阻塞首屏渲染 */
  src: url("https://your-cdn.example.com/lxgwwenkai-subset.woff2") format("woff2");
  /* ↑ 实际请用中文网字计划的 result.css，或自建子集 */
  unicode-range: U+4E00-9FFF; /* 示例：仅 CJK 基本区 */
}
```

> ⚠️ 上面 `@font-face` 的 URL 是占位示例。**实际用法是直接引中文网字计划的
> `result.css`**（它自带 269 个 `@font-face` 块和完整 `unicode-range`），而不是自己写一条。

**小程序端：**
```js
// app.js —— global: true 全局生效（基础库 2.10.0+）
wx.loadFontFace({
  family: 'LXGW WenKai',
  source: 'url("https://your-cdn.com/wenkai-subset.ttf")',  // 注意用 TTF/WOFF，勿用 WOFF2
  global: true,
  scopes: ['webview', 'native'],
  success: res => console.log('字体加载成功', res.status),
  fail: err => console.error('字体加载失败', err)
})
```

---

## 六、数字与拉丁字符搭配

### 6.1 为什么必须单独指定

霞鹜文楷、思源宋体等中文字体**自带西文部分**，但：
- 霞鹜文楷的西文来自 Klee One，**偏日系、偏窄**
- 思源宋体的西文来自 Source Serif，质量高但**偏硬**
- 游戏 UI 里的日期、数量、分数需要**等宽对齐 + 独立气质**

### 6.2 推荐搭配（✅ 均已确认在 Google Fonts 上可用）

| 西文字体 | 气质 | 与谁搭 | 备注 |
|---|---|---|---|
| **Ysabeau Office** | 温和人文衬线，低对比度 | **霞鹜文楷**（作者官方推荐） | ⭐ 首选。作者已做合并字体 **LXGW Bright** |
| **EB Garamond** | 古典书卷，优雅 | 思源宋体 / 朱雀仿宋 | 老牌开源衬线，书卷气最正 |
| **Crimson Pro** | 柔和衬线，安静 | 霞鹜文楷 | 比 Garamond 稍现代 |
| **Spectral** | 屏幕优化衬线，清晰 | 正文数字 | 小字号可读性好 |
| **Cormorant Garamond** | 极细高对比，装饰性强 | 仅大标题 | 小字号会糊 |
| **Newsreader / Literata** | 阅读型衬线 | 长文本 | Literata 是 Google 阅读器字体 |
| **Inter** | 中性无衬线 | 系统黑体正文 | 数字清晰，UI 控件友好 |
| **Fraunces** | 复古软衬线 | 标题 | 有「soft」轴，可调柔和度 |

> **📌 精致方案**：直接用 **LXGW Bright**（霞鹜文楷 + Ysabeau Office 合并字体），
> 中西文一次性统一，作者官方项目、OFL 授权，中文网字计划 CDN 上有 `lxgwwenkaibright` 包。
> 来源：https://github.com/lxgw/LxgwBright

### 6.3 数字排版细节

```css
/* 日期、数量、分数：等宽数字 + 衬线，与楷体气质统一 */
.num, time, .date, .score {
  font-family: var(--font-num);
  font-variant-numeric: tabular-nums;   /* 等宽对齐 */
  font-feature-settings: "tnum" 1;
}

/* 校名缩写等拉丁大写：加点字距更显呼吸感 */
.abbr, .en-title {
  font-family: var(--font-num);
  letter-spacing: 0.08em;
  text-transform: uppercase;   /* 谨慎使用，SJTU 这类缩写可全大写 */
}
```

---

## 七、反面清单（这个风格里**不能**用的字体）

### 7.1 「可爱风」圆体——**最大的雷区**

| 字体 | 为什么不能用 |
|---|---|
| **华康少女文字** | 极致甜腻，二次元萌系，与「安静克制」完全相反 |
| **方正少儿 / 方正字迹系列** | 儿童感强烈，会显得幼稚 |
| **站酷快乐体** | 笔画夸张跳跃，是「活泼」不是「安静」 |
| **郑庆科黄油体** | 圆胖厚重，视觉上「腻」 |
| **汉仪小麦体 / 汉仪润圆** | 甜系圆体，UI 会显廉价 |
| **Wawati SC（娃娃体，iOS 系统自带）** | 系统里就有，但**绝对不要用**——手写但过于"娃娃" |
| **猫啃网糖圆体** | 名字即风格，糖系 |
| **优设圆体 / 各种「圆体」** | 圆体的本质是「降低锐度」，但过度圆润 = 可爱 |

> 💭 **推断**：圆体在本项目里整体禁用。若一定要圆的柔和感，
> 用**字重更轻的黑体 + 更大字距 + 更多留白**来营造，而不是换圆体。

### 7.2 太锐/太重的黑体

| 字体 | 问题 |
|---|---|
| **思源黑体 Heavy / Black** | 字重过重，压迫感强，破坏「呼吸感」 |
| **阿里巴巴普惠体 Bold/Heavy** | 企业感、商务感，与校园温柔气质冲突 |
| **站酷高端黑** | 硬朗锐利，是「潮牌」不是「书卷」 |
| **OPPO Sans Bold / MiSans Bold** | 过粗会失去轻盈感（**用 Regular/Medium 即可**） |

### 7.3 手写但「太用力」的书法体

| 字体 | 问题 |
|---|---|
| **汉仪尚巍手书** | 笔锋凌厉、张力强，是「江湖气」不是「书卷气」 |
| **禹卫书法行书 / 李旭科书法** | 毛笔味过浓，像海报不像游戏 UI |
| **各种「毛笔」「狂草」字体** | 装饰性 >> 可读性，移动端小字号完全糊 |

### 7.4 ⚠️ 清松手写体的**版本陷阱**

清松手写体是一个系列，**风格差异极大**，选错就翻车：

| 版本 | 气质 | 本项目 |
|---|---|---|
| 1 | 圆润 | ❌ |
| 2 | 秀气 | ⚠️ 偏可爱 |
| 3 | 呆萌 | ❌ |
| 4 | POP | ❌ |
| **5** | **行楷** | ✅ **可用** |
| 6 | Q萌 | ❌ |
| 7 | 飘逸 | ⚠️ 偏张扬 |
| **8** | **随性** | ✅ **可用** |
| **9** | **文青** | ✅ **首选** |

> 💭 这类型号差异是「同名字体不同气质」的典型陷阱，
> 引入前务必**逐个下载看样张**，不要只看字体名。

### 7.5 其他不适合的类别

| 类别 | 例子 | 问题 |
|---|---|---|
| 像素/点阵字体 | 方正像素、Zpix | 游戏感太强，破坏柔和 |
| 手写英文花体 | Script 类 | 与中文楷体气质不搭，且可读性差 |
| 卡通标题字体 | 各类「卡通」「漫画」体 | 风格完全跑偏 |
| 过度装饰的宋体 | 部分「美术宋」 | 装饰性压过书卷气 |

---

## 八、需要用户自行确认的事项（⚠️ 汇总）

| # | 事项 | 为什么不确定 | 去哪里确认 |
|---|---|---|---|
| 1 | **京華老宋体授权** | 作者声明 vs 字体元数据 "All rights reserved" vs 部分站点称付费，**三方冲突** | 联系作者**特里王**（知乎「活字考古」）；或直接改用朱雀仿宋/思源宋体 |
| 2 | **全瀨體授权** | 第三方称 OFL 1.1，**未找到官方授权页** | cjkFonts 官方渠道 |
| 3 | **中文网字计划 CDN 域名** | 官网显示已从 `chinese-fonts-cdn.deno.dev` 迁移至 `cn-font.claude-code-best.win`，**该域名非典型 CDN 域名，请自行核验** | https://chinese-font.netlify.app/ 与 https://github.com/KonghaYao/chinese-free-web-font-storage |
| 4 | **第三方 CDN 长期可用性** | 公益项目，无 SLA，可能停服或换域名 | 正式发布**建议自建**子集（用 `cn-font-split`） |
| 5 | **清松手写体授权可能变更** | 部分收录站提示「版权方可能临时调整授权范围」 | https://github.com/jasonhandwriting/JasonHandwriting |
| 6 | **朱雀仿宋仍为测试版** | 字汇仅约 6763 字（GB2312 量级），可能缺字 | https://github.com/TrionesType/zhuque |
| 7 | **辰宇落雁體简体支持弱** | 仅约 9000 字，繁体为主，简体可能缺字 | https://github.com/Chenyu-otf/chenyuluoyan_thin |
| 8 | **霞鹜新晰黑 IPA 1.0 嵌入条款** | 官方明确要求自行评估合规成本 | https://github.com/lxgw/lxgw/blob/main/documents/xizhi_embedding_instructions.md |
| 9 | **学校层面的用字规范** | 交大可能有 VI/字体规范要求 | 校内宣传部门；另注意京華老宋体作者明确说**不要用于教育场合** |

---

## 九、最终建议方案

### 🥇 首选（气质 + 合规 + 成本 三者平衡最好）

```
标题/氛围字：霞鹜文楷 LXGW WenKai（unicode-range 子集化，按需加载）
正文/UI：  系统字体栈（零加载）
数字/西文：Ysabeau Office（或直接用 LXGW Bright 合并版）
```

**理由：**
1. **气质最准**——霞鹜文楷是基于 FONTWORKS Klee One 的教科书体，兼具楷体笔调与仿宋端正，
   正是「柔和 + 克制 + 手写感 + 书卷气」，且**不是圆体**，天然避开甜腻
2. **授权最干净**——OFL 1.1，且作者**明文额外授权 Web 子集化**（OFL 里罕见），
   这是全清单里唯一一个把 Web 字体这件事写进许可文件的
3. **工程最成熟**——有官方 npm webfont 包、有国内 CDN、作者认可的托管平台，
   不需要从零搭子集化流程
4. **风险最低**——中文网字计划若不可用，可随时回退系统字体栈，气质打折但不崩

### 🥈 备选 A（想要更「旧书/年代感」）
标题用 **朱雀仿宋**（OFL 1.1，民国活字风），正文系统字体。
避开京華老宋体的授权风险。

### 🥉 备选 B（完全不加载 Web 字体）
只用 `--font-soft-system` 系统栈。iOS 上能拿到 `Songti SC`/`Kaiti SC`，
书卷气尚可；**但 Android 上会退化成黑体，两端气质不统一**——
这是这个方案最大的妥协，需产品侧接受。

---

## 十、来源清单

**字体官方仓库 / 授权文件**
- 霞鹜文楷：https://github.com/lxgw/LxgwWenKai ｜ OFL：https://raw.githubusercontent.com/lxgw/LxgwWenKai/main/OFL.txt
- 霞鹜新晰黑（IPA 1.0）：https://github.com/lxgw/LxgwNeoXiHei
- 霞鹜 new 致宋 / Bright：https://github.com/lxgw/LxgwBright
- 悠哉字体：https://github.com/lxgw/yozai-font
- 芫荽 Iansui：https://github.com/ButTaiwan/iansui ｜ OFL：https://raw.githubusercontent.com/ButTaiwan/iansui/main/OFL.txt
- 朱雀仿宋：https://github.com/TrionesType/zhuque
- 清松手写体：https://github.com/jasonhandwriting/JasonHandwriting
- 辰宇落雁體：https://github.com/Chenyu-otf/chenyuluoyan_thin
- 更纱黑体：https://github.com/be5invis/Sarasa-Gothic ｜ LICENSE：https://raw.githubusercontent.com/be5invis/Sarasa-Gothic/master/LICENSE
- 思源宋体：https://github.com/adobe-fonts/source-han-serif
- 思源黑体：https://github.com/adobe-fonts/source-han-sans
- SIL OFL 官方 FAQ：https://openfontlicense.org

**Web 字体方案**
- 中文网字计划：https://chinese-font.netlify.app/
- 字体仓库：https://github.com/KonghaYao/chinese-free-web-font-storage
- 分包引擎：https://github.com/KonghaYao/cn-font-split
- 霞鹜文楷 webfont npm：https://github.com/chawyehsu/lxgw-wenkai-webfont
- ZSFT 字体 CDN：https://fonts.zeoseven.com/

**官方文档**
- 微信小程序 `wx.loadFontFace`：https://developers.weixin.qq.com/miniprogram/dev/api/ui/font/wx.loadFontFace.html
- Apple 系统字体列表：https://developer.apple.com/fonts/system-fonts/
- MiSans 官网：https://hyperos.mi.com/font/zh/
- Google Fonts：https://fonts.google.com/

**其他**
- 京華老宋体推荐（阮一峰周刊 Issue）：https://github.com/ruanyf/weekly/issues/4078
- 猫啃网（免费商用字体收录）：https://www.maoken.com/
