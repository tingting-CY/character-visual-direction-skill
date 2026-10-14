# 漫画分镜与非均分四宫格

> 仅在用户需要一页四格、非 2×2 布局、漫画镜头递进或主角越框时读取。目标是一页讲清一个转折；高要求页面默认逐格生成后拼版。

## 页面先于 Prompt

按以下顺序规划：

1. 用一句话确定**页级事件**，例如“角色看到信号后冲向列车”。
2. 确定阅读方向，默认左→右、上→下。
3. 分配四个节拍：**建立 → 触发 → 行动 → 反应/余韵**。
4. 选择哪个节拍面积最大；为每格指定唯一镜头职责。
5. 预留低细节区域给后期对白；不要要求图像模型生成可读文字。

## 共同规则

- 指定 `exactly four distinct bordered panels, asymmetrical layout, not a 2 by 2 grid`。
- 使用清晰的 `clean white gutters, consistent panel borders`。
- 每格最多一个主动作、一个叙事锚点和 0–1 组支持元素。
- 四格至少出现一格环境、一格动作/转折、一格信息特写和一格情绪/结果；不要四个中景站桩。
- 跨格保持角色脸、发型、服装、标志配饰、主色与主光方向一致。
- 加入 `no text, no speech bubbles, no captions, no watermark`；将文字留给后期排版。

## 最小可用布局

| 布局 | 结构 | 推荐节奏 |
|---|---|---|
| `L01` 锚点瀑布式 | 上方宽格 + 下方左侧竖格 + 右侧上下小格 | 地点 → 动作 → 信息 → 反应 |
| `L02` 纵向压迫式 | 左侧全高竖格 + 右侧三个小横格 | 巨大空间/人物压力 → 连续触发与决定 |
| `L03` 反转阶梯式 | 上方两个不等格 + 下方宽格 + 窄竖格 | 发现 → 意识 → 行动 → 余韵 |
| `L04` 中心爆发式 | 中心最大动作格 + 三个嵌入小格 | 触发信息与反应为中心动作蓄力 |

为最大格放最大事件；小格只承载手、眼、物件、信号或余波，不重复完整场景。

## 镜头递进

| 节拍 | 推荐镜头 | 作用 |
|---|---|---|
| 建立 | 35–50mm 中广景、眼平或微俯 | 交代人物、地点与初始状态 |
| 触发 | 50–85mm 物件/手部/眼神特写 | 说明发生了什么 |
| 行动 | 18–35mm 低机位、三分之四或肩后 | 推进方向与身体动势 |
| 反应/余韵 | 50–135mm 中近景、特写或背影 | 让情绪或结果落点成立 |

不要让每格都使用极端广角、荷兰角或爆炸特效；一页只允许一个最高风险镜头。

## 主角破格 / 越框

破格是**起源格中的连续角色局部**穿过边框和格间留白，不是删除格线、复制角色或把两格合并。

| 级别 | 使用方式 | 约束 |
|---|---|---|
| 微破格 | 发梢、手指、披风角进入 `gutter` 约 5–10% | 用于轻微速度或不安 |
| 中破格（默认） | 手臂、头肩、武器进入邻格边缘约 10–20% | 一页只用一次，邻格焦点必须可读 |
| 大破格 | 上半身或动作前端突破页边 | 仅最大动作格和页面高潮可用 |

使用规则：指定起源格；只突破一个元素；默认沿阅读方向突破；保留边框在越框元素后方可见；光线、色温和材质与起源格连续；不要遮挡邻格核心信息。

## 原生单页 Prompt 骨架

```text
A single [vertical / horizontal] comic page, exactly four distinct bordered panels, [L01 / L02 / L03 / L04], asymmetrical layout with uneven panel sizes, not a 2 by 2 grid, clean white gutters and consistent panel borders.

All panels feature [固定角色锚点] with consistent facial identity, costume, accessories, style, and lighting direction. One clear page event: [页级事件].
Panel 1 [位置/面积]: [建立镜头 + 场景 + 锚点].
Panel 2 [位置/面积]: [触发或动作 + 锚点].
Panel 3 [位置/面积]: [物件或反应特写].
Panel 4 [位置/面积]: [结果/余韵].

[统一主光与色彩关系]. Clear reading order from panel 1 to panel 4. No text, no speech bubbles, no captions, no watermark, no extra panels, no 2 by 2 grid.
```

需要越框时，在骨架后追加：

```text
Use one deliberate panel-break only: [起源格] contains [角色部位/道具], crossing its [方向] border through the white gutter and overlapping only [邻格] by about [10–20%]. Keep the border visibly behind it and [邻格] fully readable. This is one continuous character, not a duplicate.
```

## 工作流选择与验收

- **概念测试**：可原生生成单页；先验收格数、阅读顺序、镜头差异，再看细节。
- **正式交付**：按 P1–P4 逐格生成，以共同角色参考图锁定身份；再用排版工具加格线、越框前景和文字。
- **失败处理**：原生页变为 2×2、格数错误、角色跨格漂移或对白乱码时，改走逐格拼版；不要反复堆叠布局形容词。

验收：恰好四格、最大格承载最大事件、阅读方向清楚、镜头职责不同、每格元素不过载、角色锚点与主光一致、越框不遮挡邻格焦点。
