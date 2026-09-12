# 生图提示词参考

> ⚠️ **诚实声明**：下面的提示词模板是**从实测参数【推导】出来的，未经过实际出图验证**。我没有可用的图像生成器来跑 A/B。
> 词表结构（正向锚点 + 负面清单）是这套风格能否出来的关键，但**具体权重和措辞需要你用实际模型调**。第一轮出图后请把结果反馈回来修正本文件。

## 核心原则：负面词比正面词重要

这套风格的**定义有一半靠「不做什么」**。绝大多数图像模型「可爱卡通」的先验极强，会自动给你加上高饱和、大眼睛高光、腮红、阴影、渐变天空——**全是这套风格要删的东西**。

所以：**正面词可以少，负面词不能省。**

---

## 1. 提示词结构

```
[媒介] + [场景/主体] + [配色约束] + [线条约束] + [光照约束] + [构图约束] + [氛围词]
```

### 逐段模板

| 段 | 写什么 | 例 |
|---|---|---|
| **媒介** | 明确是扁平矢量，不是渲染 | `flat vector illustration`, `2D flat graphic` |
| **主体** | 画什么，**并且明确画小** | `a small [主体] in a wide calm [场景]` |
| **配色** | 低饱和、高明度、近灰占比 | `muted desaturated palette`, `high-key`, `warm neutral background`, `soft grey-green` |
| **线条** | 单支暗浊墨、等宽、圆头 | `uniform dark desaturated olive outline`, `consistent line weight`, `rounded line caps` |
| **光照** | **平涂**，无渐变 | `flat solid color fills`, `hard edges`, `no gradients`, `no shading` |
| **构图** | 主体小、大面积空 | `subject occupies under 2% of frame`, `vast empty area`, `minimal detail` |
| **氛围** | 箱庭／侘寂／禅 | `japanese zen garden aesthetic`, `haconiwa`, `wabi-sabi`, `quiet and restrained` |

### 完整示例（【自定】·未验证）

```
flat vector illustration, a very small frog sitting in a wide calm japanese
courtyard garden, muted desaturated palette, high-key lighting, warm sand
and cool grey neutrals, one large flat area of pale sand filling most of the
frame, uniform dark olive outline with consistent line weight and rounded caps,
flat solid color fills with hard edges, no gradients, no shading,
subject occupies under 2% of the frame, extensive empty space,
minimal facial detail, eyes as simple solid dark shapes,
japanese zen garden aesthetic, wabi-sabi, quiet restrained and still
```

---

## 2. 必须写进负面词的 8 项

每一条都对应一个会被模型自动加上、但必须删掉的东西。

| # | 负面词 | 对应的失败模式 |
|---|---|---|
| 1 | `3D render, octane render, unreal engine, blender, ray tracing, CGI` | 模型默认往 3D 走 |
| 2 | `photorealistic, photo, realistic, hyperrealistic` | 同上 |
| 3 | `watercolor, gouache, paper texture, canvas texture, film grain, noise, textured` | **最常见也最致命的错误**——中文圈普遍误以为它是水彩。实测平坦区 SD=0.00，**根本没有肌理层** |
| 4 | `gradient, soft shading, airbrush, cel shading, ambient occlusion, rendering` | 全画面平涂，唯一允许渐变的是「照片」这种另一种媒介 |
| 5 | `black outline, bold black lines, thick black border, heavy linework` | 全场零纯黑。粗黑描边会立刻变成「粗描边儿童插画」，另一个风格 |
| 6 | `sparkle eyes, shiny eyes, eye highlights, blush, rosy cheeks, eyelashes, cute anime eyes, big eyes, kawaii face` | **去糖的核心**。眼睛只能是一个实心色块 |
| 7 | `dramatic lighting, dark shadows, vignette, gloomy, moody, contrast` | 明度中位 0.77–0.89，暗端被压缩，不能有暗部 |
| 8 | `particles, magic glow, sparkles, bokeh, lens flare, light rays, glowing effects` | 开发者原话：「不会过多添加超现实的幻想要素」 |

**完整的负面提示词串（可直接复制）**：

```
3D render, octane render, unreal engine, blender, ray tracing, CGI,
photorealistic, photo, realistic, hyperrealistic,
watercolor, gouache, paper texture, canvas texture, film grain, noise,
textured, textured background,
gradient, soft shading, airbrush, cel shading, ambient occlusion, rendering,
black outline, bold black lines, thick black border, heavy linework,
sparkle eyes, shiny eyes, eye highlights, blush, rosy cheeks, eyelashes,
cute anime eyes, big eyes, kawaii face, chibi,
dramatic lighting, dark shadows, vignette, gloomy, moody, high contrast,
particles, magic glow, sparkles, bokeh, lens flare, light rays, glowing
```

---

## 3. 角色设计要点

角色的「不甜腻」是靠**删**出来的，不是靠造型：

| 部位 | 【实测】原作做法 | 提示词写法 |
|---|---|---|
| **眼睛** | **一个深色斜杏仁形实心块**。无眼白、无瞳孔、无高光、无星星眼、无睫毛 | `eyes as a single solid dark almond shape` |
| **腮红** | 无 | 负面词 `blush` |
| **嘴** | 极小色块，不夸张 | `tiny simple mouth` |
| **体型** | 约 1:1 头≈身短胖（【目测·弱证据】，头身比未精确测量） | `short plump body, head roughly equal to body` |
| **表情** | 「一脸淡定」——这是「反差萌」的来源 | `calm neutral expression, deadpan` |
| **高光** | 无 | 负面词 `highlights` |
| **投影** | 无。靠勾线分离物体 | 负面词 `drop shadow, cast shadow, ground shadow` |

**自检**：出图后放大看眼睛。如果眼睛里有**任何**白色或亮点，就是失败的——那是最容易漏掉、也最破坏风格的一处。

---

## 4. 构图要点

- **主体画小**：原文实测主角占屏 **0.49%**。提示词明确写 `subject occupies under 2% of the frame`
- **大面积空**：单色平涂块最高占屏 **43.2%**。写 `one large flat area of [色] filling most of the frame`
- **空有颜色**：留白是**同色系的暖砂／暖灰褐／冷天蓝平涂**，不是白色。写 `vast empty area of pale warm sand` 而不是 `white background`
- **无暗角、无景深**：负面词 `vignette, depth of field, blur`

---

## 5. 「照片」这一类图要单独处理

原作有一个**很值得学的招**：主画面严格平涂，但**旅行照片允许单调连续渐变**——用媒介差异讲故事（照片是「另一种媒介的产物」）。

所以如果你要生成「游戏内照片／明信片」类的图，**规则要改**：

```
... allowed: smooth monotone sky gradient, photographic framing,
white paper border, slightly warm color cast
```

并**从负面词里去掉** `gradient`。

这是全套规则里唯一的合法例外，**别让它泄漏到主画面上**。

---

## 6. 出图后自检

- [ ] 放大看眼睛：有没有白色/亮点？（有 = 失败）
- [ ] 画面里能不能找到纯黑？（有 = 失败）
- [ ] 有没有渐变、雾面、光晕？（主画面有 = 失败）
- [ ] 有没有纸纹/颗粒/噪点？（有 = 失败。这是最常见错误）
- [ ] 主体是不是很小、空的地方是不是很大？
- [ ] 「空」的地方是**有颜色的平涂**还是白色？
- [ ] 线条是不是等宽、圆头、暗浊彩色（非黑）？
- [ ] 有没有投影、高光、腮红、粒子？

**把出图结果和本文件的偏差反馈回来**——本文件是【推导】未验证的，需要靠实际出图迭代。
