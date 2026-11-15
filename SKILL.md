---
name: yudi-cover
description: 为公众号「御弟悟AI」生成水墨风格封面图。当用户需要出公众号封面、题图、系列文章封面（如"本地模型能做什么"系列），或提到御弟、封面、给文章配图出封面时使用。锁定御弟 IP 形象与 FE-061 暖纸枯笔水墨画风，每期只换系列名、主标题和画面场景。
---

# 御弟悟AI 封面生成（yudi-cover）

## 概述

生成「御弟悟AI」公众号封面：21:9（2560x1080，对应微信 2.35:1 头图）。形象与画风已锁定，每期只换三处文案 + 一个场景，保证品牌记忆不漂移。

## 资源

- `assets/character-ref.png` — 御弟形象定稿（身份硬参考，每图必带）
- `assets/style-ref.png` — 画风基准：FE-061 当代人文水墨漫画（枯笔、暖纸、书法印章）

## 输入

向用户收集三项，缺失时主动推断或追问：

1. **主标题**（必给，8-14 字为宜，封面大字）
2. **系列名**（可省，缺省则封面不出现；有则作小字 kicker，如「本地模型能做什么」。样式与主标题强制区分：朱红小楷，字号明显小，不加粗）
3. **本期场景**：御弟的一个核心动作。优先隐喻（如"给老电脑扇扇子"=低成本跑模型），主题抽象时先想画面隐喻再出图

## 生成流程

1. **想场景**：每期只画一个核心动作，御弟必须参与动作（不是角落贴纸）；电路元素优先画成藤蔓/植物/水路等自然形态。
2. **调 ImageGen**：`images` 传两张——第一张 `character-ref.png`（身份参考），第二张 `style-ref.png`（画风参考）；`size` 用 `2560x1080`；prompt 用下方模板填槽。
3. **验收**：对照下方检查清单逐项核对，不合格按"迭代纪律"一次只改一处重生成。
4. **交付**：文件落盘 `vibe_images/`，对话里给绝对路径链接；如需 1:1 方形版（视频号/朋友圈），同 prompt 加一句"central square composition, scene centered"并改尺寸重出。

## Prompt 模板

填三个槽：`{TITLE}` 主标题、`{KICKER}` 系列名（可省；无系列名则删去 Text 行里 kicker 那半句，只留主标题+印章）、`{SCENE}` 场景句。

```
Use case: wechat-official-account-cover, wide 21:9 banner.

Input images:
- Image 1 is the CHARACTER REFERENCE — the SLIM chibi monk mascot must appear exactly as in Image 1, and must stay SLIM: narrow shoulders, lean waist, straight falling robe silhouette, small hands and feet. Copy exactly: small neat five-petal gold Pilu hat (毗卢帽) with glowing cyan circuit boards, WHITE wide-sleeved robe with loose black INK-BLOT collar and lapel edging (brushy ink-wash blots, NOT solid black trim) and ink-wash accent blots on sleeves and hem, thin ochre sash, chip prayer beads, round smiling face. Do NOT make him chubby or round-bodied.
- Image 2 is the STYLE AND ATMOSPHERE REFERENCE — borrow its brushwork, rendering AND background elements: rough dry-brush Chinese ink strokes, bold calligraphy title, ink-wash grey tones on warm paper, sparse color accents, vermilion seals, loose humorous literati cartoon feel, AND its faded ink-wash distant mountains at the frame edges with a small ink bamboo branch at the upper right corner. Do NOT copy Image 2's composition or its little scholar figure.

Composition: left third mostly blank warm paper holding text; right two thirds the scene, low along the bottom edge with lots of empty paper above; faded ink-wash distant mountains rising at the far right edge behind the scene and a faint mountain corner at the lower left, one small ink bamboo branch at the upper right, exactly the atmospheric backdrop style of Image 2.

Scene: {SCENE}
Text (verbatim, no extra characters, each line appears exactly once): large title "{TITLE}" in bold expressive black Chinese calligraphy like Image 2; smaller kicker above it "{KICKER}" in neat small REGULAR script (小楷, thin even strokes, no cursive links) in VERMILION RED, noticeably smaller and NOT bold, clearly differentiated from the title in both color and style; a square vermilion seal stamp "御弟悟AI" at lower left in INTAGLIO style (阴文/白文): solid red square with a thin paper-white hairline inner border frame, two rows of bold paper-white carved characters 御弟 / 悟AI reversed out of the red ground, NOT red characters on white.
Constraints: light warm paper background, NOT dark; rough dry-brush ink style like Image 2, not smooth watercolor; faded ink-wash mountains and bamboo kept in background; character identical to Image 1 and STAYS SLIM, not chubby; no photorealism, no watermark, no extra text.
```

## 验收清单

- 文案齐全、逐字正确、各出现一次：主标题与印章必有；有系列名时系列名为朱红小楷小字、与主标题明显区分；无系列名时不出现系列名字样
- 印章为阴文（白文）：红底实心方块、内一道纸白细线框、上排御弟下排悟AI 反白加粗，非红字白底
- 御弟清瘦：窄肩、直身，非胖和尚
- 衣领是水墨斑块皴染，非死黑色块
- 白袍为底、袖口下摆有墨块；小五叶电路毗卢帽、芯片念珠在
- 暖纸底（非深色底）、枯笔飞白（非光滑水彩）
- 远景山水墨 + 右上墨竹在场
- 无鼠标等未指定道具

## 迭代纪律（实测教训）

- **一次只改一处**，改完重述其余不变项。
- 文案重复/错字 → 用 precise-text-edit 单独修文字，不动画面。
- 体型漂移变胖 → prompt 里显式加 `STAYS SLIM, not chubby` + 参考图放第一位。
- 衣领变成死黑 → 强调 `ink-wash blots, NOT solid black trim`。
- 墨块过多盖掉白袍 → 指定 `white-dominant, blots about one-third coverage`。
- 印章变阳文（红字白底）或丢了内框白线 → 强调 `INTAGLIO (阴文/白文): solid red square with a thin paper-white inner border frame, bold paper-white characters 御弟 / 悟AI reversed out`。
- 系列名写成行书/草书或与主标题同色同重 → 强调 `neat small REGULAR script (小楷), vermilion red, not bold`。
- 出图永远带两张参考图，漏带必漂移。
