# 漫画分镜与非均分四宫格布局

> 用于生成单页漫画、四宫格叙事页、非 2×2 面板布局、角色分镜探索和动作节奏页。重点不是让画面“出现四个格子”，而是让**格子面积、阅读顺序、镜头距离、留白和叙事信息量**共同形成节奏。

## 适用与边界

### 适合

- 用户提出“四宫格漫画”“一页四格”“分镜页”“不想要死板 2×2”“一张图讲一个转折”等需求。
- 需要以不同镜头展示同一角色的环境、动作、反应和结果。
- 需要用宽格、竖格、小格、嵌入格或斜格控制叙事节奏。
- 需要在不改变角色锚点的前提下，生成有镜头递进的漫画页面。

### 不适合或应转为其他工作流

- 需要准确可编辑的对话文本、气泡排版、页码、分镜编号或像素级格线：先逐格生成，再使用设计/排版工具拼版和叠加文字。
- 需要角色跨四格完全一致、手部稳定或复杂动作连续：优先逐格生成并用共同角色参考图，最后按布局拼版。
- 需要技术分镜、动画镜头表或严格工程坐标：应输出表格/分镜表，而不是要求图像模型生成最终漫画页。

> **重要限制**：原生图像生成适合做“概念性单页漫画”，但对准确的四格数量、格线几何、重复角色一致性和可读文字不稳定。高要求交付默认采用“逐格生成 → 排版组装”流程。

## 先做页面导演，而不是先写 Prompt

每一页只承载**一个小型叙事转折**。用以下顺序做规划：

1. **确定阅读方向**：默认左→右、上→下；若用户明确要求日式漫画阅读，改为右→左、上→下，并同步镜像布局与镜头指向。
2. **定义页级事件**：用一句话描述“角色在什么时刻发生了什么转折”。例如“快递员在站台收到求救信号，回头并冲向列车”。
3. **选择四个叙事节拍**：建立 / 触发 / 反应或动作 / 落点。不要让四格都只是在不同姿势中站立。
4. **选择非均分布局**：先决定哪个节拍应拥有最大面积，再按动线分配其他三格。
5. **为每格分配一个镜头职责**：环境、动作、物件、反应、结果；每格只保留一个主镜头意图。
6. **决定留白与文本安全区**：若后续要添加文字，先为气泡或标题留出单色、低细节区域；不要要求图像模型生成可读台词。

## 四宫格页面的基础语法

| 版面语言 | 叙事含义 | 使用建议 |
|---|---|---|
| 宽格 / 横向大格 | 建立地点、时间停顿、环境尺度、重大揭示 | 放在开场或结尾；它不一定需要画满道具 |
| 竖向大格 | 压迫、坠落、上升、人物与空间的纵向关系 | 适合高楼、站台、角色冲刺、悬崖或角色对峙 |
| 小横格 | 时间加速、物件特写、连续动作、表情切换 | 通常用于大格旁的“插入节拍” |
| 小竖格 | 视线切换、局部动作、紧张停顿 | 不应承载过多环境信息 |
| 嵌入格 / inset | 信息强调、记忆、物件、眼神或瞬间反应 | 页面概念图可用；正式成品建议后期拼版 |
| 斜格 / 不规则格 | 危机、冲击、混乱、速度 | 仅在情绪有理由时使用；不要让每个格子都倾斜 |
| 出血格 / 无边框格 | 时间突破、巨大动作、情绪爆发 | 四格内最多一个；其余格子保持边界以维持可读性 |
| 宽留白 / 宽沟槽 | 停顿、孤独、悬念、时间跳跃 | 沟槽本身也是节奏，不是浪费空间 |

## 通用页面约束

无论选择哪种布局，均遵守：

- **恰好四格**：在 Prompt 中明确 `exactly four distinct comic panels`，并指出各格位置和相对大小。
- **不使用 2×2 等分**：在 Prompt 中明确 `asymmetrical layout, not a 2 by 2 grid, uneven panel sizes`。
- **格间留白清楚**：指定 `clean white gutters, consistent panel borders`；除非选择出血格，否则格子不能互相覆盖。
- **页面只含一个事件**：四格围绕同一角色、同一时空或明确的短时跳跃。
- **一格一职能**：每格最多一个主动作、一个叙事锚点和零到一组支持元素。
- **镜头有递进**：默认采用“宽景 / 动作或物件 / 特写反应 / 结果或余韵”，而不是四个同样的中景肖像。
- **不生成文字**：添加 `no text, no speech bubbles, no captions, no watermark`；需要对白时留空白区后期叠字。
- **保持锚点**：角色脸、发型、服装、关键配饰、主色和光线方向应跨格一致，除非剧情明确发生变化。

## 四种经典的非均分四宫格布局

以下坐标以竖版页面 `100 × 100` 的相对面积表示，`gutter` 为格间留白。坐标可用于后期拼版；原生生成时用相对关系描述即可，不必要求模型严格执行像素数值。

### L01：锚点瀑布式 `1 + 1 + 2`

**结构**：上方一幅横向大建立格；下方左侧一幅竖向动作格；右侧上下两个小横格。

```text
┌──────────────────────────────┐
│             P1               │
├────────────────┬─────────────┤
│                │     P3      │
│       P2       ├─────────────┤
│                │     P4      │
└────────────────┴─────────────┘
```

| 格 | 相对比例 | 推荐职责 | 推荐镜头 |
|---|---|---|---|
| P1 | 宽 `100%` × 高约 `46%` | 地点、人物处境、事件前的平静 | 35mm 眼平或微俯的中广景 |
| P2 | 宽约 `56%` × 高约 `50%` | 主动作、姿态或环境接触 | 24–35mm 低机位 / 三分之四角度 |
| P3 | 宽约 `40%` × 高约 `23%` | 物件、手部、视线或触发信息 | 50–85mm 特写 |
| P4 | 宽约 `40%` × 高约 `23%` | 脸部反应或结果落点 | 50–85mm 特写或中近景 |

**叙事节奏**：建立 → 行动 → 触发细节 → 情绪落点。

**适合**：角色在某地点收到信号、发现物件、触发决定或从静止进入行动。

### L02：纵向压迫式 `1 + 3`

**结构**：左侧竖向大格占据整页高度；右侧三个横向小格自上而下排列。

```text
┌──────────────┬───────────────┐
│              │      P2       │
│              ├───────────────┤
│      P1      │      P3       │
│              ├───────────────┤
│              │      P4       │
└──────────────┴───────────────┘
```

| 格 | 相对比例 | 推荐职责 | 推荐镜头 |
|---|---|---|---|
| P1 | 宽约 `42%` × 高 `100%` | 巨大空间、坠落、奔跑、对峙或角色纵向姿态 | 18–35mm，低机位、虫眼或强俯视 |
| P2 | 右侧高约 `30%` | 看到 / 听到的触发信息 | 85mm 物件或眼神特写 |
| P3 | 右侧高约 `32%` | 即刻反应、手部或姿态改变 | 50mm 中近景 |
| P4 | 右侧高约 `30%` | 决定、结果、方向性动作 | 35–50mm 侧面或肩后 |

**叙事节奏**：空间压力持续存在，右侧以三个细小节拍推进时间。

**适合**：角色站在高层边缘、穿过狭长通道、面对巨物、从静到动的追逐或压迫场景。

### L03：反转阶梯式 `2 + 1 + 1`

**结构**：上方两个比例不等的格子；下方左侧一个宽大结果格；下方右侧一个窄竖格作为悬念或余韵。

```text
┌───────────┬──────────────────┐
│    P1     │        P2        │
├──────────────────────┬───────┤
│          P3          │  P4   │
└──────────────────────┴───────┘
```

| 格 | 相对比例 | 推荐职责 | 推荐镜头 |
|---|---|---|---|
| P1 | 上左小格 | 物件、眼神或异常信号 | 85mm 特写 |
| P2 | 上右宽格 | 角色意识到异常 | 50mm 中近景或肩后 |
| P3 | 下左宽格 | 动作、选择或空间结果 | 24–35mm 中广景 |
| P4 | 下右窄格 | 余韵、背影、手机屏幕、手部或沉默 | 85mm 竖向特写 |

**叙事节奏**：小发现 → 人物意识 → 大动作 / 转折 → 余波。

**适合**：悬疑、反转、角色做出决定后的静默，或以一个细节收束页面。

### L04：中心爆发与三个嵌入节拍 `1 + 3 inset`

**结构**：一幅中心大动作格为页面主体，周围或边缘放置三个较小嵌入格。正式交付优先后期拼版；原生生成仅用于概念草图。

```text
┌──────────┬───────────────────┐
│    P2    │                   │
├──────────┤        P1         │
│    P3    │    central hero   │
├──────────┤                   │
│    P4    │                   │
└──────────┴───────────────────┘
```

| 格 | 相对比例 | 推荐职责 | 推荐镜头 |
|---|---|---|---|
| P1 | 中心最大格 | 核心动作、爆发、冲刺、跌落或揭示 | 18–28mm、低机位、透视缩短或出血动作 |
| P2 | 小嵌入格 | 触发物或环境预警 | 85mm 特写 |
| P3 | 小嵌入格 | 眼神或手部反应 | 85mm 特写 |
| P4 | 小嵌入格 | 结果细节、物件落地或余波 | 50–85mm 特写 |

**叙事节奏**：用三次短促信息为中央动作蓄力或回响。

**适合**：动作爆发、魔法施放、逃离、跌落、临界选择。

**注意**：一个页面只允许 P1 使用出血或强透视；小格保持简单、清晰、水平，避免全页“每格都在吼叫”。

## 主角破格与越框（Panel Break）

**破格 / 越框**是指角色的头部、手臂、武器、披风或身体局部有意识地穿过其所属画格的边框和 `gutter`，形成前景层级与爆发感。它不是删除所有格线，也不是多画一个独立人物；其本质是“**一个来自某格的连续主体，暂时覆盖格线**”。

### 何时使用

| 叙事时刻 | 适合的突破部位 | 建议布局 |
|---|---|---|
| 角色做出决定或突然转身 | 头部、肩膀、前手 | `L01` P2 或 `L03` P3 |
| 冲刺、攻击、抓取或救援 | 手臂、腿、武器、披风尾部 | `L01` P2、`L04` P1 |
| 角色压迫感或登场 | 头部、上半身跨出上边框 | `L02` P1、`L04` P1 |
| 物件触发或信息爆发 | 手持物、信号、光束的小范围越框 | `L01` P3、`L03` P1 |
| 页面高潮或翻页钩子 | 角色身体或武器的一部分突破外页边缘 | `L04` P1；仅限页末或海报式页面 |

不要把破格用在基准角色图、服装设定图、平静对话或每一个动作场景。它应标记一个真正的动作、选择或情绪高点。

### 突破幅度与层级

| 级别 | 覆盖范围 | 适用效果 | 约束 |
|---|---|---|---|
| 微破格 | 手指、发梢、披风角跨过边框并进入 `gutter` 的约 `5–10%` | 轻微不安、速度、细小情绪 | 不遮挡邻格焦点；适合连续页中的小强调 |
| 中破格 | 手臂、头肩、武器的一部分进入 `gutter` 并覆盖邻格边缘约 `10–20%` | 决定、转身、攻击、强反应 | 一页只用一次；需清楚标明来自哪一格 |
| 大破格 | 上半身、前景武器或动态披风跨出格线，局部突破页面外边缘 | 登场、爆发、翻页钩子 | 仅限最大动作格；其他三格保持稳定 |

默认使用**中破格**。让角色的脸或核心动作仍主要属于起源格，只有最有方向性的部位越框；这样既能产生前景冲击，也不会让读者失去“该角色正在这一格发生什么”的判断。

### 设计规则

1. **指定起源格**：明确写“从 P2 突破”或“从 P1 突破”。越框对象只能来自一个画格，不能在两个格中复制同一角色。
2. **只突破一个元素**：一个页面最多一次越框；选择头肩、前手、武器或披风中的一项主元素，不让四肢、道具、粒子同时跨格。
3. **沿阅读方向突破**：默认左→右阅读时，越框动作优先向右或向下推进；需要“逆流、阻挡、回头”时才向左突破，并让剧情解释这种逆向感。
4. **保留格线结构**：边框应在越框对象后方可见，`gutter` 仍能被识别；越框不是把两个格合并成一幅大图。
5. **保护邻格焦点**：被覆盖的邻格最多损失边缘 `10–20%`，其物件、眼神或叙事信息必须保持可读。
6. **保持光线连续**：越框部分的光线、色温、材质与起源格完全一致；不要让跨出的手臂突然使用邻格的光源。
7. **限制外页出血**：只有页面高潮才让主体突破画布外边缘；普通四格优先突破内部格线和 `gutter`。

### 原生生成 Prompt 写法

在原生单页 Prompt 的全局布局描述后加入以下句式，并替换方括号：

```text
Use one deliberate panel-break effect only: [起源格] contains the [角色部位 / 道具], which crosses its [方向] panel border, extends through the white gutter, and overlaps only the [邻格] edge by approximately [10–20%]. The panel border remains visibly behind the crossing element; [邻格] remains fully readable. This is one continuous character, not a duplicate and not an extra panel.
```

可搭配的避免项：

```text
no extra panels, no duplicated character, no disconnected floating limbs, no merged panels, no broken reading order, no cluttered overlapping objects
```

### 逐格拼版做法（推荐）

高要求交付时，将破格视为额外的前景图层，而不是要求单张图一次完成：

1. 按选定布局生成 P1–P4 的基础画面，先不绘制边框和气泡。
2. 单独生成或从起源格提取“越框主体”，例如角色的前手、头肩或武器；保持同一角色参考、镜头、光线和动作连续性。
3. 在排版工具中先放置四个基础画面与 `gutter`，再将越框主体置于格线之上、对白和 UI 文字之下。
4. 只允许越框主体覆盖一个相邻格的边缘；检查邻格的叙事锚点和阅读方向仍然清楚。
5. 最后添加气泡和字幕，避免文字压在越框主体的脸部、手部或动作路径上。

这种方法比原生单页生成更适合需要精确格线、可编辑文字、角色一致性和多轮修改的团队生产。

### C04：L01 锚点瀑布式——主角突破 P2 格线

**页级事件**：快递员在雨夜站台看到紧急信号，从 P2 的行动格冲出，前手越过格线进入 P3 的信息格。

```text
A single vertical 4:5 comic page, exactly four distinct bordered panels, L01 anchor-cascade layout: one wide top panel, one tall lower-left panel, and two smaller stacked panels on the lower right. Asymmetrical layout with uneven panel sizes, not a 2 by 2 grid, clean white gutters and consistent thin panel borders.

All four panels feature the same 27-year-old East Asian female courier with a short black bob, a small silver hair clip on the left side, amber eyes, a dark navy cropped courier jacket with reflective seams, an orange messenger bag, black cargo trousers, worn boots, and a vintage communication watch on her left wrist. Keep her facial identity, costume, rainy-night teal-and-amber semi-realistic cinematic science-fiction concept-art style, and lighting direction consistent across all panels.

Panel 1, wide top establishing panel: eye-level medium-wide shot, 35mm lens. She stands on a rain-wet elevated transit platform beneath one cool cyan station light; only blurred rails appear in the background.
Panel 2, tall lower-left action panel: knee-level camera, upward tilt around 15 degrees, three-quarter view, 24mm lens. She launches forward from the wet platform, one boot pushing off the ground, messenger bag and jacket trailing behind her.
Panel 3, small upper-right detail panel: 85mm close-up of the communication watch with an abstract red emergency signal, no readable text.
Panel 4, small lower-right reaction panel: 85mm close-up portrait, controlled urgency in her eyes, looking toward the arriving train.

Use one deliberate panel-break effect only: Panel 2 contains the courier's extended right arm and hand, which cross Panel 2's upper-right border, extend through the white gutter, and overlap only the lower-left edge of Panel 3 by approximately 15%. The Panel 2 border remains visibly behind the arm; Panel 3's emergency signal remains fully readable. This is one continuous courier, not a duplicate and not an extra panel.

Cool cyan key light from the left, restrained amber train rim light from behind, wet pavement reflections and physically consistent contact shadows. Clear reading order from panel 1 to panel 4. No text, no speech bubbles, no captions, no watermark, no extra panels, no 2 by 2 grid, no duplicated character, no disconnected floating limbs, no merged panels.
```

**验收**：P2 是越框起源，前手以一条连续动作线进入 P3；P3 的通信器仍可读，P4 不被遮挡；页面仍然是明确的四格而不是“P2 与 P3 合并的一张大图”。

## 镜头递进模板

非均分布局不会自动带来叙事性。为每个布局先安排镜头节奏：

| 页面功能 | 典型镜头 | 目的 |
|---|---|---|
| 建立 | 35–50mm，眼平或微俯，中广景 | 交代地点、人物位置和初始状态 |
| 触发 | 50–85mm，物件 / 手部 / 眼神特写 | 让观众知道“发生了什么” |
| 行动 | 18–35mm，低机位、三分之四或肩后 | 让身体方向和空间线条推进阅读 |
| 反应 | 50–85mm，稳定中近景或特写 | 让情绪成为落点，而非继续堆动效 |
| 余韵 | 50–135mm，背影、静物或留白 | 留下悬念、孤独、结果或下一页钩子 |

默认情况下，一个四格页至少包含：**一格环境/空间镜头 + 一格动作或转折 + 一格信息特写 + 一格情绪或结果**。

## 工作流选择

### 路线 A：原生单页生成

用于概念图、快速尝试和不含可读文字的漫画页。

1. 选择 `L01–L04` 中一个布局。
2. 在 Prompt 首句锁定页面规格：`a single vertical comic page, exactly four distinct bordered panels, [布局名], asymmetrical layout, not a 2 by 2 grid`。
3. 按 `P1 → P2 → P3 → P4` 明确写每格的位置、面积、镜头、角色动作和叙事功能。
4. 指定 `clean white gutters, consistent panel borders, no text, no speech bubbles, no captions`。
5. 生成后先验收格数、阅读顺序和镜头差异，再评价脸部与细节。

**适用预期**：整体节奏、画面气氛、布局概念。

**不适用预期**：精确的格线比例、稳定的角色重复、可读文字、复杂连续动作的最终成品。

### 路线 B：逐格生成后拼版（推荐）

用于团队交付、人物一致性、复杂动作和需要后续加入对白的页面。

1. 先制作并锁定角色基准图。
2. 选择 `L01–L04` 布局，建立页面规格和每格坐标。
3. 以共同角色基准图分别生成 P1–P4；每格只携带该格的镜头与场景信息。
4. 对每格进行身份、姿态、光影和叙事锚点验收；修复单格，不重做全页。
5. 在设计或排版工具中按坐标放入四张图，加入统一边框和 `gutter`。
6. 最后叠加对白、旁白、音效和页码；文字由排版工具生成，不由图像模型生成。

**适用预期**：精确格数、非矩形边界、角色稳定、可编辑文字、可复现的团队生产。

## 页面 Prompt 骨架

### 原生单页通用模板

```text
A single [vertical / horizontal] comic page, exactly four distinct bordered panels, [L01 / L02 / L03 / L04 布局名称], asymmetrical layout with uneven panel sizes, not a 2 by 2 grid, clean white gutters and consistent panel borders.

All four panels feature [固定角色锚点] with consistent facial identity, hairstyle, costume, signature accessories, rendering style, and lighting direction. The page tells one clear beat: [页级事件].

Panel 1 [位置与面积]: [镜头参数], [动作或地点], [叙事锚点], [最多 1 组支持元素].
Panel 2 [位置与面积]: [镜头参数], [动作或触发], [叙事锚点].
Panel 3 [位置与面积]: [镜头参数], [反应或物件特写].
Panel 4 [位置与面积]: [镜头参数], [结果、情绪或余韵].

[统一主光源与色彩关系], [画风与材质]. Clear reading order from panel 1 to panel 4. No text, no speech bubbles, no captions, no watermark, no extra panels, no 2 by 2 grid.
```

### 逐格生成 Prompt 骨架

```text
Comic page [布局名], panel [P1 / P2 / P3 / P4]. Preserve the same character identity as the approved reference: [固定角色锚点].

[此格的景别、相机高度、Pitch、Yaw、Roll、焦段和相机距离].
[此格唯一的动作、叙事锚点和零到一组支持元素].
[与全页统一的光线方向、色温、风格和材质].
Reserve [上方 / 左侧 / 右侧] low-detail negative space for later dialogue overlay. No text, no speech bubbles, no watermark.
```

## 实际 Prompt 示例

以下三例均沿用同一个角色锚点，以验证布局、镜头递进和四格叙事。可将角色描述替换为项目角色，并将同一角色参考图附加到每轮生成。

### C01：L01 锚点瀑布式——站台求救信号

**页级事件**：雨夜站台上，快递员收到求救信号，看到信息后做出追赶列车的决定。

```text
A single vertical 4:5 comic page, exactly four distinct bordered panels, L01 anchor-cascade layout: one wide top panel, one tall lower-left panel, and two smaller stacked panels on the lower right. Asymmetrical layout with uneven panel sizes, not a 2 by 2 grid, clean white gutters and consistent thin panel borders.

All four panels feature the same 27-year-old East Asian female courier with a short black bob, a small silver hair clip on the left side, amber eyes, a dark navy cropped courier jacket with reflective seams, an orange messenger bag, black cargo trousers, worn boots, and a vintage communication watch on her left wrist. Keep her facial identity, costume, teal-and-amber semi-realistic cinematic science-fiction concept-art style, and rainy-night lighting consistent across all panels.

Panel 1, wide top establishing panel: eye-level medium-wide shot, 35mm lens, camera roll 0 degrees. She stands alone on a rain-wet elevated transit platform beneath one cool cyan station light; the platform is the narrative anchor, with only blurred rails as the support group.
Panel 2, tall lower-left action panel: knee-level camera, upward tilt around 15 degrees, three-quarter view, 24mm lens. She pivots sharply toward the arriving train, one boot pushing off the wet platform, messenger bag swinging behind her.
Panel 3, small upper-right detail panel: 85mm close-up of the communication watch in her hand, its cracked screen showing an abstract red emergency signal with no readable text.
Panel 4, small lower-right reaction panel: 85mm close-up portrait, eyes widened with controlled urgency, raindrops on her cheek, looking toward the train.

Cool cyan key light from the left, restrained amber train rim light from behind, wet pavement reflections and physically consistent contact shadows. Clear reading order from panel 1 to panel 4. No text, no speech bubbles, no captions, no watermark, no extra panels, no 2 by 2 grid.
```

**验收**：P1 交代空间，P2 产生动作张力，P3 给出触发信息，P4 落在表情；四格不能都使用同样的中景。若原生生成变成 2×2，改用路线 B 拼版。

### C02：L02 纵向压迫式——高塔边缘

**页级事件**：快递员站在高层塔楼外的维修梯上，听到通信器警报，最终向下望去。

```text
A single vertical 4:5 comic page, exactly four distinct bordered panels, L02 vertical-pressure layout: one tall full-height panel on the left and three narrow horizontal panels stacked on the right. Asymmetrical layout with uneven panel sizes, not a 2 by 2 grid, clean white gutters and consistent panel borders.

All four panels feature the same 27-year-old East Asian female courier with a short black bob, a small silver hair clip on the left side, amber eyes, a dark navy cropped courier jacket with reflective seams, an orange messenger bag, black cargo trousers, worn boots, and a vintage communication watch on her left wrist. Preserve the same face, costume, semi-realistic cinematic science-fiction concept-art style, and cold dawn lighting across panels.

Panel 1, tall left hero panel: worm's-eye view from near the maintenance ladder, upward tilt around 35 degrees, 18mm lens. The courier clings to a vertical tower ladder against an immense foggy city shaft; the ladder is the narrative anchor, with only distant defocused windows as support.
Panel 2, narrow top-right panel: 85mm close-up of the communication watch vibrating against her glove, an abstract warning icon with no readable text.
Panel 3, narrow middle-right panel: 50mm medium close-up, side profile at 90 degrees, camera roll 0 degrees. Her jaw tightens as she hears the alert, one hand gripping the ladder rung.
Panel 4, narrow bottom-right panel: high-angle view looking down around 35 degrees, 35mm lens. Her boot pauses above the enormous drop below, leaving dark negative space under the ladder.

Cold dawn key light from above, faint warm hazard light reflected from the tower wall, consistent wind lifting the jacket hem. Clear reading order from panel 1 to panel 4. No text, no speech bubbles, no captions, no watermark, no extra panels, no 2 by 2 grid.
```

**验收**：P1 用竖格形成高度与压迫；P2–P4 是时间推进，而不是三张同构的小肖像。若角色被极端广角拉坏，P1 改 24mm 或逐格生成后修复。

### C03：L03 反转阶梯式——发现与余韵

**页级事件**：快递员发现一枚陌生徽章，意识到信息来源，冲出站台后只留下徽章的特写作为悬念。

```text
A single vertical 4:5 comic page, exactly four distinct bordered panels, L03 reversal-stair layout: a small panel at upper left, a wider panel at upper right, a large wide panel at lower left, and a narrow vertical panel at lower right. Asymmetrical layout with uneven panel sizes, not a 2 by 2 grid, clean white gutters and consistent panel borders.

All four panels feature the same 27-year-old East Asian female courier with a short black bob, a small silver hair clip on the left side, amber eyes, a dark navy cropped courier jacket with reflective seams, an orange messenger bag, black cargo trousers, worn boots, and a vintage communication watch on her left wrist. Maintain the same facial identity, costume, semi-realistic cinematic science-fiction concept-art style, and muted rain-afterglow lighting across all panels.

Panel 1, small upper-left panel: 85mm close-up of a small unfamiliar silver badge lying in a rain puddle, reflected cyan light, no readable emblem or text.
Panel 2, wide upper-right panel: eye-level medium close-up, 50mm lens, three-quarter view at 35 degrees. The courier kneels beside the badge, her face changing from confusion to recognition.
Panel 3, large lower-left panel: ground-level camera, upward tilt around 12 degrees, 28mm lens. She runs away from the platform exit, messenger bag trailing behind, the station doorway as the narrative anchor and only one blurred support group beyond it.
Panel 4, narrow lower-right panel: 85mm vertical close-up of the badge now held between her wet fingers, broad low-detail dark space above it reserved for a later caption overlay.

Soft cool light from the left, faint warm reflections from the station interior, wet ground reflections and consistent shadow directions. Clear reading order from panel 1 to panel 4. No text, no speech bubbles, no captions, no watermark, no extra panels, no 2 by 2 grid.
```

**验收**：P1 是发现，P2 是意识，P3 是行动，P4 是悬念；P4 的窄竖格应有留白，而不是塞入另一个完整场景。

## 漫画页验收清单

| 维度 | 通过标准 | 常见失败 | 优先修复方式 |
|---|---|---|---|
| 格数与布局 | 恰好四格，布局符合 L01–L04，明显不是 2×2 | 多出格子、格线消失、所有格等大 | 改用逐格生成后拼版；不要反复叠加更多布局形容词 |
| 阅读方向 | 视线可从 P1 自然移到 P4 | 视线跳跃、主动作方向相互冲突 | 调整角色朝向、引导线、亮度和格子大小 |
| 面积权重 | 最大格承担最大事件；小格承载细节或反应 | 小格塞环境、大格没有叙事功能 | 重新分配镜头职责，不先增加道具 |
| 镜头递进 | 至少有环境、动作、信息、反应或余韵的差异 | 四个中景站桩肖像 | 指定每格不同的景别/焦段/机位职责 |
| 角色一致性 | 脸、发型、服装、配饰和主色跨格一致 | 每格像不同角色 | 使用共同基准图；改走逐格生成流程 |
| 场景密度 | 每格只含一个锚点与零到一组支持元素 | 每格都塞满城市、人群和特效 | 将世界观信息合并成背景组或转到下一页 |
| 文字安全区 | 要叠字的位置低细节、对比适中 | AI 生成乱码、气泡遮住脸 | 禁用 AI 文字；后期用排版工具叠加 |

## 常见错误与快速修正

| 错误 | 原因 | 快速修正 |
|---|---|---|
| 生成结果仍是规整 2×2 | 只写了“四宫格”，未描述面积和位置 | 指定 L01–L04 的文字结构；若仍不稳，逐格拼版 |
| 四个格子内容完全相同 | 没为每格分配镜头职责 | 使用“建立 / 触发 / 动作 / 反应”的节拍表 |
| 页面过满、阅读困难 | 每格都重复完整场景 | 每格重置元素预算：角色或物件 + 1 锚点 + 0–1 组支持元素 |
| 格线全是倾斜的、很杂乱 | 把“漫画感”误解为全页失衡 | 至多一个斜格或一个荷兰角；其余格子保持稳定 |
| 人物跨格不一致 | 单图生成器无法稳定多次呈现同一角色 | 使用参考图与逐格流程；必要时局部修复 |
| 对白变乱码 | 图像模型不擅长排版文字 | 只留空白安全区，后期加气泡和文字 |

## 团队交付记录

使用逐格流程时，在任务记录中保留：

```text
页面编号：
阅读方向：
布局：L01 / L02 / L03 / L04
页级事件：
角色基准版本：
统一光影与色彩：
P1：镜头参数 / 叙事职责 / 文件版本
P2：镜头参数 / 叙事职责 / 文件版本
P3：镜头参数 / 叙事职责 / 文件版本
P4：镜头参数 / 叙事职责 / 文件版本
拼版规格：画幅 / gutter / 边框 / 文字安全区
验收结论：
```
